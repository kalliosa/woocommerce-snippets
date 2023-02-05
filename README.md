#Enable Half Quantity
function custom_quantity_input_args( $args, $product ) {
    $args['min_value'] = 0.5;
    $args['step'] = 0.5;
    return $args;
}
add_filter( 'woocommerce_quantity_input_args', 'custom_quantity_input_args', 10, 2 );