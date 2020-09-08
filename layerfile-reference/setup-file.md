# SETUP FILE

`SETUP FILE [file ...]`

The `SETUP FILE` instruction causes the contents of the given file to be sourced before every `RUN` command.
This is equivalent to copy/pasting the contents of the file into the terminal before every `RUN` command.

A common use case is to set a lot of environment variables using an ".env" file, or specifying a custom ".bashrc" file.

### Examples

- Use `SETUP FILE .env` to run `source (repository root)/.env` before every `RUN` command.
