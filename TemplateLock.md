# Template lock feature

* [reference](https://developer.wordpress.org/block-editor/reference-guides/block-api/block-templates/)


```
function myplugin_register_template() {
    $post_type_object = get_post_type_object( 'post' );
    $post_type_object->template = array(
        array( 'core/paragraph', array(
            'placeholder' => 'Add Description...',
        ) ),
    );
    $post_type_object->template_lock = 'all';
}
add_action( 'init', 'myplugin_register_template' );

```