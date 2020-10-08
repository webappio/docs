# WORKDIR

`WORKDIR [directory]`

The `WORKDIR` instruction changes the location from which files are resolved in the runner.

### Examples

- Use `WORKDIR /tmp` to run commands in the `/tmp` directory within the runner.
- Use `WORKDIR hello` to run commands in the `(workdir)/hello` directory within the runner.
