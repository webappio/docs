#ENV 

`ENV [key=value...]`

The `ENV` instruction persistently sets environment variables in this Layerfile

### Examples

- `ENV PATH=$GOPATH/bin:$PATH` adds `$GOPATH/bin` to the existing path.
- `ENV CI=hello` sets the variable `$CI` to the value `hello`.
