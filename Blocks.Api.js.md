# Interacting with Blocks via JS (React)

## Get a list of all registered blocks
* On editor page, open browser developer console and type:
```
wp.blocks.getBlockTypes() // get all blocks, or
wp.blocks.getBlockTypes().filter((block) => { return block.name.indexOf('generateblocks') !== -1}); // this lists generateblocks only

```
[source](https://github.com/WordPress/gutenberg/issues/12775#issuecomment-793369394)