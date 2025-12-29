## which -a shows every match on PATH
- Running `which -a <command>` prints every executable that the shell will find on your `PATH`, in the order it would be chosen.
- Handy when multiple copies of a tool are installed (e.g., Codex CLI via both Bun and npm) so you can confirm which install is first in line.
- Lets you pick the exact binary you want (or adjust `PATH`) when the default choice isn't the one you expected.
