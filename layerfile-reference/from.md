# FROM 

`FROM [source]`

The `FROM` instruction tells LayerCI what base to use to run tests from.

There can only be one `FROM` line in a Layerfile, and it must always be the first directive in the Layerfile.

For now, only `FROM vm/ubuntu:18.04` is allowed as a top level, but inheriting from other Layerfiles is possible.

### Examples

- Use `FROM vm/ubuntu:18.04` to use ubuntu:18.04 as the base.
- Use `FROM ../base` to inherit from the file at `../base/Layerfile` relative to the current Layerfile
- Use `FROM /base` to inherit from the file at `(repo root)/base/Layerfile)`
