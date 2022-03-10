# SETUP FILE

`SETUP FILE [file ...]`

The `SETUP FILE` instruction causes the contents of the given file to be sourced before every `RUN` command.
This is equivalent to copy/pasting the contents of the file into the terminal before every `RUN` command.

A common use case is to set a lot of environment variables using an ".env" file, or specifying a custom ".bashrc" file.

### Example setup-script.sh

```shell
# contents of setup-script.sh
echo 'This will print before every RUN command'
# set the LOG_LEVEL environment variable to 'debug'
export LOG_LEVEL=debug
# load an .env file
source /root/.env
```

### Referencing setup-script.sh

Use `SETUP FILE setup-script.sh` to run `source setup-script.sh` before every `RUN` command.

<br />