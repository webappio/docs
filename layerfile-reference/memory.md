# MEMORY

`MEMORY [number](K|M|G)`

The `MEMORY` instruction allows you to reserve memory before you need it.
In particular, languages like `nodejs` might require memory to be available before they are used.

We'll automatically add memory as it's requested, adding memory with `MEMORY` will decrease snapshot speed.

There's a limit to an additional `4G` of memory added at once.

### Examples

- Use `MEMORY 2G` to ensure at least 2 gigabytes of memory are available.
