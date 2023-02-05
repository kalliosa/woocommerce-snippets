## Allow Half Quantity
```php
function custom_quantity_input_args( $args, $product ) {
    if ( $product->is_type( 'your_product_class' ) ) {
        $args['min_value'] = 0.5;
        $args['step'] = 0.5;
    }
    return $args;
}
add_filter( 'woocommerce_quantity_input_args', 'custom_quantity_input_args', 10, 2 );
```