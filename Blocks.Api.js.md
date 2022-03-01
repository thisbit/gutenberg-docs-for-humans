# Interacting with Blocks via JS (React)

## Get a list of all registered blocks

- On editor page, open browser developer console and type:

```
wp.blocks.getBlockTypes() // get all blocks, or
wp.blocks.getBlockTypes().filter((block) => { return block.name.indexOf('generateblocks') !== -1}); // this lists generateblocks only

```

[source](https://github.com/WordPress/gutenberg/issues/12775#issuecomment-793369394)

## Get the properties and values of a block selected in the editor

On the editor page select the block you want toe explore and paste the following in console

```
wp.data.select('core/block-editor').getSelectedBlock().attributes;

```

If you fill in all available settings in the editor, this will return these values and these can then be used in templating or similar
