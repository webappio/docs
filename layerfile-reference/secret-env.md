# SECRET ENV

`SECRET ENV [secret name...]`

The `SECRET ENV` instruction adds values from secrets to the runner's environment.
It's useful for authenticating LayerCI with other services on your behalf.

### Examples

- Use `SECRET ENV ENV_FILE` to expose your dotfile env `.env` and then use `RUN echo "$ENV_FILE" | base64 -d > ~/.env` to decode the uploaded env file to the specific location.
