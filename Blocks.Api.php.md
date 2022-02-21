# Interacting with Blocks via PhP (hooks and filters)
## Whitelist blocks

```
function thisbit_allowed_block_types( $allowed_blocks ) {
 
	global $post;
	$allowed_blocks = array( // Paragraph is the only block that is available everywhere for everyone.
		'core/paragraph'
	);
 
	if ( $post->post_type === 'page' ) { // Image block is available for everyone in pages.
		$allowed_blocks[] = 'core/image';
	}

	if ( current_user_can( 'customize' ) ) { // Headings are available for admins everywhere.
		$allowed_blocks[] = 'core/heading';
	}
	return $allowed_blocks;
}

add_filter( 'allowed_block_types_all', 'thisbit_allowed_block_types', 10, 2 ); // Works with 5.9 if < 5.9 try the allowed_block_types hook.
```
Not Good: this way no one can use 3rd party blocks
Good: If we allow editors to edit pages, and if admins have placed headings before hand, editors cannot duplicate or add more headins, they can only move them arround and edit their content.

### Sources 
* [Roles & Capabilities](https://wordpress.org/support/article/roles-and-capabilities/), I refer to base this on capabilities rather then user roles, as this is in my view more flexible as it leaves us options to set various capabilities to user roles as we please, we can have custom capabilities etc.
* Remove default Blocks by [Misha Rudrastyh](https://rudrastyh.com/gutenberg/remove-default-blocks.html)

## GenerateBlocks (or other 3rd party blocks)
The $allowed_blocks array does not take 3rd party blocks at the moment, so if for example one wants to allow admins to be able to use generateblocks (or rather, allow all blocks core and 3rd party) while others can use only a limited number of core blocks ...

```
function thisbit_allowed_block_types( $allowed_blocks ) {
 
	global $post;
 
	// Depending on user capabilites
	if ( ! current_user_can( 'customize' ) ) { // If not administrator
			if ( $post->post_type === 'page' ) { // If not admin is on page post type
			$allowed_blocks = array(
				'core/image',
				'core/paragraph',
				'core/list'
			);
		} else { // If not admin is on other post types 
			$allowed_blocks = array(
				'core/paragraph'
			);
		}
	}
	return $allowed_blocks;
}

add_filter( 'allowed_block_types_all', 'thisbit_allowed_block_types', 10, 2 );

```
Not good: The problem with the second solution is that if we want to allow 3rd party blocks, we cannot put any limits on any other blocks, because as soon as we use allowed_blocks on admin, 3rd party blocks are gone.

## Combining with post type limitations
If applicable it is probably better to limit access to post types, and then limit blocks per post types.
* [Example](https://gist.github.com/thisbit/94f585cd1b3b96a0a10bd5c20dce868e)