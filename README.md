# wordpress_help
Plugins that help Wordpress Developer

## Wordpress Theme and Plugin Snippet 


## Creating a List of Available WooCommerce Coupon Codes and Display anywhere Using Shortcode
from ( https://stackoverflow.com/questions/62230388/creating-a-list-of-available-woocommerce-coupon-codes-and-display-anywhere-using )
add_shortcode('ac', 'available_coupon_codes' );
function available_coupon_codes() {
    global $wpdb;

    // Get an array of all existing coupon codes
    $coupon_codes = $wpdb->get_col("SELECT post_name FROM $wpdb->posts WHERE post_type = 'shop_coupon' AND post_status = 'publish' ORDER BY post_name ASC");

    // Display available coupon codes
    return implode(', ', $coupon_codes) ; // always use return in a shortcode
}
Code goes in functions.php file of your active child theme (or active theme). Tested and works.

USAGE:

1) In the WordPress text editor of a post, a custom post or a page:

[ac]
2) On a php file or template:

echo available_coupon_codes();
or

echo do_shortcode('[ac]');
With a WP_Query (like in your code):

add_shortcode('ac', 'coupon_list' );
function coupon_list() {
    $coupon_posts = get_posts( array(
        'posts_per_page'   => -1,
        'orderby'          => 'name',
        'order'            => 'asc',
        'post_type'        => 'shop_coupon',
        'post_status'      => 'publish',
    ) );

    $coupon_codes = []; // Initializing

    foreach( $coupon_posts as $coupon_post) {
        $coupon_codes[] = $coupon_post->post_name;
    }

    // Display available coupon codes
    return implode(', ', $coupon_codes) ; // always use return in a shortcode
}
Code goes in functions.php file of your active child theme (or active theme). Tested and works.

Same usage than the first function
