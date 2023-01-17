# Learning Kotlin Coroutines    

## AsyncTask old way, resize an image
```
class ImageResizerTask(private val callback: (Bitmap) -> Unit) : AsyncTask<String, Void, Bitmap>() {

    override fun doInBackground(vararg params: String?): Bitmap {
        val path = params[0]
        val options = BitmapFactory.Options().apply {
            inJustDecodeBounds = true
        }
        BitmapFactory.decodeFile(path, options)
        val imageHeight = options.outHeight
        val imageWidth = options.outWidth
        val scaleFactor = calculateScaleFactor(imageWidth, imageHeight)

        BitmapFactory.Options().apply {
            inSampleSize = scaleFactor
        }.let {
            return BitmapFactory.decodeFile(path, it)
        }
    }

    private fun calculateScaleFactor(width: Int, height: Int): Int {
        val targetWidth = 600
        val targetHeight = 600
        val scaleFactor = min(width / targetWidth, height / targetHeight)
        return if (scaleFactor < 1) 1 else scaleFactor
    }
}

```

## Coroutines way of doing the same thing
```
class ImageResizer {
    suspend fun resize(path: String): Bitmap {
        return withContext(Dispatchers.IO) {
            val options = BitmapFactory.Options().apply {
                inJustDecodeBounds = true
            }
            BitmapFactory.decodeFile(path, options)
            val imageHeight = options.outHeight
            val imageWidth = options.outWidth
            val scaleFactor = calculateScaleFactor(imageWidth, imageHeight)

            BitmapFactory.Options().apply {
                inSampleSize = scaleFactor
            }.let {
                BitmapFactory.decodeFile(path, it)
            }
        }
    }
    private fun calculateScaleFactor(width: Int, height: Int): Int {
        val targetWidth = 600
        val targetHeight = 600
        val scaleFactor = min(width / targetWidth, height / targetHeight)
        return if (scaleFactor < 1) 1 else scaleFactor
    }
}
```

## request Using traditional Android development
```
val url = "https://example.com"
val request = Request.Builder().url(url).build()
val client = OkHttpClient()
val response = client.newCall(request).execute()
val responseBody = response.body()?.string()
```

## Using Kotlin coroutines
```
val response = withContext(Dispatchers.IO) {
    val client = OkHttpClient()
    val request = Request.Builder().url(url).build()
    client.newCall(request).execute()
}
val responseBody = response.body()?.string()
```
