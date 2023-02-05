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

## Decimal inventory and product quantity for specific product category
```php
// Defined quantity arguments
add_filter( 'woocommerce_quantity_input_args', 'custom_quantity_input_args', 9000, 2 );
function custom_quantity_input_args( $args, $product ) {
    $product_categories = wp_get_post_terms( $product->get_id(), 'product_cat', array( 'fields' => 'ids' ) );
    if(in_array(20, $product_categories)):
      if( ! is_cart() ) {
          $args['input_value'] = 1; // Starting value
      }
      $args['min_value']   = 0.5; // Minimum value
      $args['step']        = 0.5; // Quantity steps

      return $args;
    endif;
}

// For Ajax add to cart button (define the min value)
add_filter( 'woocommerce_loop_add_to_cart_args', 'custom_loop_add_to_cart_quantity_arg', 10, 2 );
function custom_loop_add_to_cart_quantity_arg( $args, $product ) {
  $product_categories = wp_get_post_terms( $product->get_id(), 'product_cat', array( 'fields' => 'ids' ) );
    if(in_array(20, $product_categories)):
      $args['quantity'] = 0.5; // Min value

      return $args;
    endif;
}

// For product variations (define the min value)
add_filter( 'woocommerce_available_variation', 'filter_wc_available_variation_price_html', 10, 3);
function filter_wc_available_variation_price_html( $data, $product, $variation ) {
  $product_categories = wp_get_post_terms( $product->get_id(), 'product_cat', array( 'fields' => 'ids' ) );
    if(in_array(20, $product_categories)):
      $data['min_qty'] = 0.5;

      return $data;
    endif;
}

// Enable decimal quantities (in frontend and backend)
remove_filter('woocommerce_stock_amount', 'intval');
add_filter('woocommerce_stock_amount', 'floatval');
```