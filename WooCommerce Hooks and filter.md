
## **Declare WooCommerce support in third party theme**

A very important snippet that you can use to add WooCommerce support to any WordPress theme:

function mytheme_add_woocommerce_support() {  
	add_theme_support( 'woocommerce' );  
	add_theme_support( 'wc-product-gallery-zoom' );  
	add_theme_support( 'wc-product-gallery-lightbox' );  
	add_theme_support( 'wc-product-gallery-slider' );  
}   
add_action( 'after_setup_theme', 'mytheme_add_woocommerce_support' );

## Adding Custom Currency to WooCommerce

WooCommerce by default To add a custom currency in WooCommerce 2.0+, copy and paste this code in your theme functions.php file and swap out the currency code and symbol with your own. After saving changes, it should be available from your WooCommerce settings.

add_filter( ‘woocommerce_currencies’, ‘add_my_currency’ );   
function add_my_currency( $currencies ) {   
 $currencies[‘ABC’] = __( ‘Currency name’, ‘woocommerce’ );   
 return $currencies;  
}add_filter(‘woocommerce_currency_symbol’, ‘add_my_currency_symbol’, 10, 2);   
function add_my_currency_symbol( $currency_symbol, $currency )   
{   
 switch( $currency ) {   
 case ‘ABC’:   
 $currency_symbol = ‘$’;   
 break;   
 }   
 return $currency_symbol;  
}

## Remove product meta on single product page

remove_action( ‘woocommerce_single_product_summary’, ‘woocommerce_template_single_meta’, 40 );

## Remove zero decimals in product price

add_filter( ‘woocommerce_price_trim_zeros’, ‘__return_true’ );

## Hide quantity on cart page

function remove_quantity_column( $return, $product ) {  
 if ( is_cart() ) return true;  
}  
add_filter( 'woocommerce_is_sold_individually', 'remove_quantity_column', 10, 2 );

## Limit woocommerce order note length

add_filter( 'woocommerce_checkout_fields', 'limit_order_note_length' );  
function limit_order_note_length( $fields ) {  
 $fields['order']['order_comments']['maxlength'] = 200;  
 return $fields;  
}

## Show custom billing checkout fields by product id

add_action( 'woocommerce_checkout_fields', 'hqhowdotcom_cutom_checkout_field_conditional_logic' );function hqhowdotcom_cutom_checkout_field_conditional_logic( $fields ) {foreach( WC()->cart->get_cart() as $cart_item ){  
     $product_id = $cart_item['product_id'];//change 2020 to your product id  
   if( $product_id == 2020 ) {  
    $fields['billing']['billing_field_' . $product_id] = array(  
     'label'     => __('Custom Field on Checkout for ' . $product_id, 'woocommerce'),  
     'placeholder'   => _x('Custom Field on Checkout for ' . $product_id, 'placeholder', 'woocommerce'),  
     'required'  => false,  
     'class'     => array('form-row-wide'),  
     'clear'     => true  
    );  
   }}// Return checkout fields.  
 return $fields;}

## Hide all shipping method but free shipping

In the user experience, you should automatically apply the free shipping method whenever possible, which helps customers feel more comfortable with your purchase.

The code snippet below will help you do this:

function only_show_free_shipping_when_available( $rates, $package ) {  
 $new_rates = array();  
 foreach ( $rates as $rate_id => $rate ) {  
  // Only modify rates if free_shipping is present.  
  if ( 'free_shipping' === $rate->method_id ) {  
   $new_rates[ $rate_id ] = $rate;  
   break;  
  }  
 }if ( ! empty( $new_rates ) ) {  
  //Save local pickup if it's present.  
  foreach ( $rates as $rate_id => $rate ) {  
   if ('local_pickup' === $rate->method_id ) {  
    $new_rates[ $rate_id ] = $rate;  
    break;  
   }  
  }  
  return $new_rates;  
 }return $rates;  
}add_filter( 'woocommerce_package_rates', 'only_show_free_shipping_when_available', 10, 2 );

## Remove product tab on single product page

add_filter( ‘woocommerce_product_tabs’, ‘remove_product_tabs’, 98 );function remove_product_tabs( $tabs ) {  
 // Remove the additional information tab  
 unset( $tabs[‘additional_information’] );   
 return $tabs;  
}

## **Add a new country to countries list**

To add a new country to the countries list, use this snippet inside the function.php file of your theme folder:

function woo_add_my_country( $country ) {  
 $country[“AEDUB”] = ‘Dubai’;  
 return $country;  
}add_filter( ‘woocommerce_countries’, ‘woo_add_my_country’, 10, 1 );

## **Remove the breadcrumbs**

If you want to remove the breadcrumbs, here is the quick snippet to help you remove woocommerce breadcrumbs from your pages:

add_action( ‘init’, ‘remove_wc_breadcrumbs’ );  
function remove_wc_breadcrumbs() {  
   remove_action( ‘woocommerce_before_main_content’, ‘woocommerce_breadcrumb’, 20, 0 );  
}

## **Replace shop page title**

Using this block of code you can quickly replace the title of your shop. Just substitute the return value with your preferred name.

add_filter( ‘woocommerce_page_title’, ‘shop_page_title’);  
function shop_page_title($title ) {  
 if(is_shop()) {  
 return “My new title”;  
 }  
 return $title;  
}

## **Redirect to checkout page after add to cart**

To improve the sales conversions , you can automatically redirect to checkout page after adding product to cart by using the following code:

add_action( ‘add_to_cart_redirect’, ‘add_to_cart_checkout_redirect’, 16 );  
function add_to_cart_checkout_redirect() {  
    global $woocommerce;  
    $checkout_url = $woocommerce->cart->get_checkout_url();  
    return $checkout_url;  
}

## **Remove product categories from shop page**

If you want to get rid of a certain product category from your shop page, this code is very useful. The code will hide all the products from the mentioned categories.

add_action( ‘pre_get_posts’, ‘remove_categories_shop’ );function remove_categories_shop( $q ) {if ( ! $q->is_main_query() ) return;if ( ! $q->is_post_type_archive() ) return;if ( ! is_admin() && is_shop() && ! is_user_logged_in() ) {$q->set( ‘tax_query’, array(array(‘taxonomy’ => ‘product_cat’,‘field’ => ‘slug’,// Don’t display products in these categories on the shop page  
 ‘terms’ => array( ‘color’, ‘flavor’, ‘spices’, ‘vanilla’ ),‘operator’ => ‘NOT IN’)));  
 }remove_action( ‘pre_get_posts’, ‘remove_categories_shop’ );}

## Removing company name from WooCommerce checkout

To remove the company name field from the WooCommerce checkout, all you need is use the hook  **woocommerce_checkout_fields**  and then apply a filter to unset the [billing] [billing_company] field from the array returned.

add_filter( ‘woocommerce_checkout_fields’ , ‘remove_company_name’ ); function remove_company_name( $fields ) {   
   unset($fields[‘billing’][‘billing_company’]);   
   return $fields;  
}

Note: You can also unset other fields using the same method. Here is another example:

add_filter( ‘woocommerce_checkout_fields’ , ‘custom_remove_woo_checkout_fields’ );  
   
function custom_remove_woo_checkout_fields( $fields ) {// remove billing fields  
 unset($fields[‘billing’][‘billing_first_name’]);  
 unset($fields[‘billing’][‘billing_last_name’]);  
 unset($fields[‘billing’][‘billing_company’]);  
 unset($fields[‘billing’][‘billing_address_1’]);  
 unset($fields[‘billing’][‘billing_address_2’]);  
 unset($fields[‘billing’][‘billing_city’]);  
 unset($fields[‘billing’][‘billing_postcode’]);  
 unset($fields[‘billing’][‘billing_country’]);  
 unset($fields[‘billing’][‘billing_state’]);  
 unset($fields[‘billing’][‘billing_phone’]);  
 unset($fields[‘billing’][‘billing_email’]);  
   
 // remove shipping fields   
 unset($fields[‘shipping’][‘shipping_first_name’]);   
 unset($fields[‘shipping’][‘shipping_last_name’]);   
 unset($fields[‘shipping’][‘shipping_company’]);  
 unset($fields[‘shipping’][‘shipping_address_1’]);  
 unset($fields[‘shipping’][‘shipping_address_2’]);  
 unset($fields[‘shipping’][‘shipping_city’]);  
 unset($fields[‘shipping’][‘shipping_postcode’]);  
 unset($fields[‘shipping’][‘shipping_country’]);  
 unset($fields[‘shipping’][‘shipping_state’]);  
   
 // remove order comment fields  
 unset($fields[‘order’][‘order_comments’]);  
   
 return $fields;  
}

## Remove the state field in the WooCommerce Checkout

function remove_state_field( $fields ) {  
 unset( $fields[‘state’] );  
 return $fields;  
}  
add_filter( ‘woocommerce_default_address_fields’, ‘remove_state_field’ );

## Quickly translate any string

add_filter('gettext',  'translate_text');  
add_filter('ngettext',  'translate_text');  
   
function translate_text($translated) {  
     $translated = str_ireplace('Choose and option',  'Select',  $translated);  
     return $translated;  
}

## Exclude a category from the WooCommerce category widget

add_filter( 'woocommerce_product_categories_widget_args', 'woo_product_cat_widget_args' );  
  
function woo_product_cat_widget_args( $cat_args ) {  
	  
	$cat_args['exclude'] = array('16');  
	  
	return $cat_args;  
}

## Replace “Out of stock” by “sold”

add_filter('woocommerce_get_availability', 'availability_filter_func');  
  
function availability_filter_func($availability)  
{  
	$availability['availability'] = str_ireplace('Out of stock', 'Sold', $availability['availability']);  
	return $availability;  
}

## Display “product already in cart” instead of “add to cart” button

/**  
 * Change the add to cart text on single product pages  
 */  
add_filter( 'woocommerce_product_single_add_to_cart_text', 'woo_custom_cart_button_text' );  
  
function woo_custom_cart_button_text() {  
  
	global $woocommerce;  
	  
	foreach($woocommerce->cart->get_cart() as $cart_item_key => $values ) {  
		$_product = $values['data'];  
	  
		if( get_the_ID() == $_product->id ) {  
			return __('Already in cart - Add Again?', 'woocommerce');  
		}  
	}  
	  
	return __('Add to cart', 'woocommerce');  
}  
  
/**  
 * Change the add to cart text on product archives  
 */  
add_filter( 'add_to_cart_text', 'woo_archive_custom_cart_button_text' );  
  
function woo_archive_custom_cart_button_text() {  
  
	global $woocommerce;  
	  
	foreach($woocommerce->cart->get_cart() as $cart_item_key => $values ) {  
		$_product = $values['data'];  
	  
		if( get_the_ID() == $_product->id ) {  
			return __('Already in cart', 'woocommerce');  
		}  
	}  
	  
	return __('Add to cart', 'woocommerce');  
}

## Hide products count in category view

add_filter( 'woocommerce_subcategory_count_html', 'woo_remove_category_products_count' );  
  
function woo_remove_category_products_count() {  
	return;  
}

## Make account checkout fields required

add_filter( 'woocommerce_checkout_fields', 'woo_filter_account_checkout_fields' );  
   
function woo_filter_account_checkout_fields( $fields ) {  
	$fields['account']['account_username']['required'] = true;  
	$fields['account']['account_password']['required'] = true;  
	$fields['account']['account_password-2']['required'] = true;  
  
	return $fields;  
}

## Rename a product tab

add_filter( 'woocommerce_product_tabs', 'woo_rename_tab', 98);  
function woo_rename_tab($tabs) {  
  
 $tabs['description']['title'] = 'More info';  
  
 return $tabs;  
}

## Add a custom field to a product variation
```
//Display Fields  
add_action( 'woocommerce_product_after_variable_attributes', 'variable_fields', 10, 2 );  
//JS to add fields for new variations  
add_action( 'woocommerce_product_after_variable_attributes_js', 'variable_fields_js' );  
//Save variation fields  
add_action( 'woocommerce_process_product_meta_variable', 'variable_fields_process', 10, 1 );  
  
function variable_fields( $loop, $variation_data ) { ?>	  
	<tr>  
		<td>  
			<div>  
					<label></label>  
					<input type="text" size="5" name="my_custom_field[]" value=""/>  
			</div>  
		</td>  
	</tr>  
  
<tr>  
		<td>  
			<div>  
					<label></label>  
					  
			</div>  
		</td>  
	</tr>  
<?php }  
   
function variable_fields_process( $post_id ) {  
	if (isset( $_POST['variable_sku'] ) ) :  
		$variable_sku = $_POST['variable_sku'];  
		$variable_post_id = $_POST['variable_post_id'];  
		$variable_custom_field = $_POST['my_custom_field'];  
		for ( $i = 0; $i < sizeof( $variable_sku ); $i++ ) :  
			$variation_id = (int) $variable_post_id[$i];  
			if ( isset( $variable_custom_field[$i] ) ) {  
				update_post_meta( $variation_id, '_my_custom_field', stripslashes( $variable_custom_field[$i] ) );  
			}  
		endfor;  
	endif;  
}

```
## List WooCommerce product Categories

$args = array(  
    'number'     => $number,  
    'orderby'    => $orderby,  
    'order'      => $order,  
    'hide_empty' => $hide_empty,  
    'include'    => $ids  
);  
  
$product_categories = get_terms( 'product_cat', $args );  
  
$count = count($product_categories);  
 if ( $count > 0 ){  
     echo "<ul>";  
     foreach ( $product_categories as $product_category ) {  
       echo '<li><a href="' . get_term_link( $product_category ) . '">' . $product_category->name . '</li>';  
          
     }  
     echo "</ul>";  
 }

## Change “from” email address

function woo_custom_wp_mail_from_name() {  
        global $woocommerce;  
        return html_entity_decode( get_option( 'woocommerce_email_from_name' ) );  
}  
add_filter( 'wp_mail_from_name', 'woo_custom_wp_mail_from_name', 99 );  
  
function woo_custom_wp_mail_from() {  
        global $woocommerce;  
        return html_entity_decode( get_option( 'woocommerce_email_from' ) );  
}  
add_filter( 'wp_mail_from_name', 'woo_custom_wp_mail_from_name', 99 );

## Return featured products ids

function woo_get_featured_product_ids() {  
	// Load from cache  
	$featured_product_ids = get_transient( 'wc_featured_products' );  
  
	// Valid cache found  
	if ( false !== $featured_product_ids )  
		return $featured_product_ids;  
  
	$featured = get_posts( array(  
		'post_type'      => array( 'product', 'product_variation' ),  
		'posts_per_page' => -1,  
		'post_status'    => 'publish',  
		'meta_query'     => array(  
			array(  
				'key' 		=> '_visibility',  
				'value' 	=> array('catalog', 'visible'),  
				'compare' 	=> 'IN'  
			),  
			array(  
				'key' 	=> '_featured',  
				'value' => 'yes'  
			)  
		),  
		'fields' => 'id=>parent'  
	) );  
  
	$product_ids = array_keys( $featured );  
	$parent_ids  = array_values( $featured );  
	$featured_product_ids = array_unique( array_merge( $product_ids, $parent_ids ) );  
  
	set_transient( 'wc_featured_products', $featured_product_ids );  
  
	return $featured_product_ids;  
}

## Set minimum order amount

add_action( 'woocommerce_checkout_process', 'wc_minimum_order_amount' );  
function wc_minimum_order_amount() {  
	global $woocommerce;  
	$minimum = 50;  
	if ( $woocommerce->cart->get_cart_total(); < $minimum ) {  
           $woocommerce->add_error( sprintf( 'You must have an order with a minimum of %s to place your order.' , $minimum ) );  
	}  
}

## Order by price, date or title on shop page

add_filter('woocommerce_default_catalog_orderby', 'custom_default_catalog_orderby');  
   
function custom_default_catalog_orderby() {  
     return 'date'; // Can also use title and price  
}

## Add email recipient when order completed

function woo_extra_email_recipient($recipient, $object) {  
    $recipient = $recipient . ', your@email.com';  
    return $recipient;  
}  
add_filter( 'woocommerce_email_recipient_customer_completed_order', 'woo_extra_email_recipient', 10, 2);

## Make phone number not required

add_filter( 'woocommerce_billing_fields', 'wc_npr_filter_phone', 10, 1 );  
function wc_npr_filter_phone( $address_fields ) {  
 $address_fields['billing_phone']['required'] = false;  
 return $address_fields;  
}

## Adding Custom Fields To Emails

To use this code, follow these steps:

1.  Add this snippet to your theme’s functions.php file
2.  Change the meta key names in the snippet
3.  Create a custom field in the order post — e.g. key = “Tracking Code” value = abcdefg
4.  When next updating the status, or during any other event which emails the user, they will see this field in their email

add_filter(‘woocommerce_email_order_meta_keys’, ‘my_custom_order_meta_keys’);function my_custom_order_meta_keys( $keys ) {  
 $keys[] = ‘Tracking Code’; // This will look for a custom field called ‘Tracking Code’ and add it to emails  
 return $keys;  
}

## Adding a Custom Field to Checkout page
```
Let’s add a new field to checkout, after the order notes, by hooking into the following:

add_action( 'woocommerce_after_order_notes', 'my_custom_checkout_field' );function my_custom_checkout_field( $checkout ) {echo '<div id="my_custom_checkout_field"><h2>' . __('My Field') . '</h2>';woocommerce_form_field( 'my_field_name', array(  
        'type'          => 'text',  
        'class'         => array('my-field-class form-row-wide'),  
        'label'         => __('Fill in this field'),  
        'placeholder'   => __('Enter something'),  
        ), $checkout->get_value( 'my_field_name' ));echo '</div>';}

Next we need to validate the field when the checkout form is posted. For this example the field is required and not optional:

add_action('woocommerce_checkout_process', 'my_custom_checkout_field_process');function my_custom_checkout_field_process() {  
    // Check if set, if its not set add an error.  
    if ( ! $_POST['my_field_name'] )  
        wc_add_notice( __( 'Please enter something into this new shiny field.' ), 'error' );  
}

Finally, let’s save the new field to order custom fields using the following code:

add_action( 'woocommerce_checkout_update_order_meta', 'my_custom_checkout_field_update_order_meta' );function my_custom_checkout_field_update_order_meta( $order_id ) {  
    if ( ! empty( $_POST['my_field_name'] ) ) {  
        update_post_meta( $order_id, 'My Field', sanitize_text_field( $_POST['my_field_name'] ) );  
    }  
}
```
## Add Content Under “Place Order” Button at WooCommerce Checkout

add_action( 'woocommerce_review_order_after_submit', 'bbloomer_privacy_message_below_checkout_button' );  
   
function bbloomer_privacy_message_below_checkout_button() {  
   echo '<p><small>Some Text Here</small></p>';  
}

## Add Text Before and After Add to Cart

// Before Add to Cart Button: Can be done easily with woocommerce_before_add_to_cart_button hook, example:add_action( ‘woocommerce_before_add_to_cart_button’, ‘before_add_to_cart_btn’ );  
   
function before_add_to_cart_btn(){  
   echo ‘Some custom text here’;  
}// After Add to Cart Button: If you are going to add some custom text after “Add to Cart” button, woocommerce_after_add_to_cart_button hook should help you. Example of usage this hookadd_action( ‘woocommerce_after_add_to_cart_button’, ‘after_add_to_cart_btn’ );  
   
function after_add_to_cart_btn(){  
    echo ‘Some custom text here’;  
}

## Reorder Checkout Fields in WooCommerce
```
First thing you have to keep in mind, that fields are separated into groups, and actually there are 4 groups:

-   billing — Billing Address
-   shipping — Shipping Address
-   account — Account Login
-   order — Additional information

Each of these groups contains fields, I think you know which ones. And you can super easily reorder them with a special priority parameter.

Example — I would like to make the email field the first one to display, I can do it with these couple lines of code:

add_filter( ‘woocommerce_checkout_fields’, ‘email_first’ );  
   
function email_first( $checkout_fields ) {  
 $checkout_fields[‘billing’][‘billing_email’][‘priority’] = 4;  
 return $checkout_fields;  
}

Just by setting the priority lower in number, as the lowest number is 10, so we made it 4 for email so it becomes the first field.

Here are the priority number list for billing fields:

-   billing billing_first_name 10
-   billing_last_name 20
-   billing_company 30
-   billing_country 40
-   billing_address_1 50
-   billing_address_2 60
-   billing_city 70
-   billing_state 80
-   billing_postcode 90
-   billing_phone 100
-   billing_email 110
```
## Check if Product Belongs to a Product Category or Tag

if( has_term( 4, ‘product_cat’ ) ) {  
 // do something if current product in the loop is in product category with ID 4  
}if( has_term( array( ‘sneakers’, ‘backpacks’ ), ‘product_cat’, 50 ) {  
 // do something if product with ID 50 is either in category “sneakers” or “backpacks”  
} else {  
 // do something else if it isn’t  
}if( has_term( 5, ‘product_tag’, 971 ) ) {  
 // do something if product with ID = 971 has tag with ID = 5  
}

## Change number of products display in WooCommerce product listing page

/**  
 * Change number of products that are displayed per page (shop page)  
 */  
add_filter( 'loop_shop_per_page', 'new_loop_shop_per_page', 20 );  
  
function new_loop_shop_per_page( $cols ) {  
  // $cols contains the current number of products per page based on the value stored on Options -> Reading  
  // Return the number of products you wanna show per page.  
  $cols = 9;  
  return $cols;  
}

## Add custom check boxes fields above the terms and conditions in WooCommerce checkout

add_action('woocommerce_checkout_before_terms_and_conditions', 'checkout_additional_checkboxes');  
function checkout_additional_checkboxes( ){  
    $checkbox1_text = __( "My first checkbox text", "woocommerce" );  
    $checkbox2_text = __( "My Second checkbox text", "woocommerce" );  
    ?>  
    <p class="form-row custom-checkboxes">  
        <label class="woocommerce-form__label checkbox custom-one">  
            <input type="checkbox" class="woocommerce-form__input woocommerce-form__input-checkbox input-checkbox" name="custom_one" > <span><?php echo  $checkbox1_text; ?></span> <span class="required">*</span>  
        </label>  
        <label class="woocommerce-form__label checkbox custom-two">  
            <input type="checkbox" class="woocommerce-form__input woocommerce-form__input-checkbox input-checkbox" name="custom_two" > <span><?php echo  $checkbox2_text; ?></span> <span class="required">*</span>  
        </label>  
    </p>  
    <?php  
}add_action('woocommerce_checkout_process', 'my_custom_checkout_field_process');  
function my_custom_checkout_field_process() {  
    // Check if set, if its not set add an error.  
    if ( ! $_POST['custom_one'] )  
        wc_add_notice( __( 'You must accept "My first checkbox".' ), 'error' );  
    if ( ! $_POST['custom_two'] )  
        wc_add_notice( __( 'You must accept "My second checkbox".' ), 'error' );  
}

## Extend admin fields in WooCommerce orders page

Sometimes you need to further extend the Admin Listing. For instance in the current problem i needed to add the serial numbers to the WooCommerce orders listing.

1.  `edit_shop_order_columns`  is the function where i am rearranging the fields

2.  `shop_order_column`  is the function where i am managing those fields and providing the values respectively.

add_filter( 'manage_edit-shop_order_columns', 'edit_shop_order_columns' ) ;  
function edit_shop_order_columns( $columns ) {  
 $columns = array(  
  'cb' => '&lt;input type="checkbox" />',  
  'order_number' => __( 'Order' ),  
  'order_date' => __( 'Order Date' ),  
  'order_status' => __( 'Status' ),  
  'serial' => __( 'Serial Number' ),  
  'order_total' => __( 'Order Total'),  
 );  
 return $columns;  
}  
add_action( 'manage_shop_order_posts_custom_column', 'shop_order_column', 10, 2);function shop_order_column( $column, $post_id ) {  
 global $post;  
 $arr = "";  
 if ( 'serial' === $column ) {  
  echo get_field('order_serial_number',$post_id);  
 }  
}add_filter( 'manage_edit-serialnumber_columns', 'edit_serialnumber_columns' );function edit_serialnumber_columns( $columns ) {$columns = array(  
 'cb' => '&lt;input type="checkbox" />',  
 'title' => __( 'Serial Numner' ),  
 'products' => __( 'Linked Products' ),  
 'SKU' => __( 'SKU' ),  
 'status' => __( 'Status'),  
 'date' => __( 'Added Date' )  
);return $columns;  
}add_action( 'manage_serialnumber_posts_custom_column', 'realestate_column', 10, 2);  
function realestate_column( $column, $post_id ) {global $post;  
   
 $arr = "";  
 if ( 'products' === $column ) {  
  $productlist = get_field('products',$post_id);  
  foreach($productlist as $row) {  
   $arr[] = get_the_title($row);  
  }  
  echo implode(",",$arr);  
 }  
   
 $sku = "";  
 if ( 'SKU' === $column ) {  
  $productlist = get_field('products',$post_id);  
  foreach($productlist as $row) {  
   $product = wc_get_product( $row );  
   echo $row;  
   $sku[] = $product->get_sku();  
  }  
  echo implode(",",$sku);  
 }if ( 'status' === $column ) {  
  echo get_field('available',$post_id);  
 }}

## Disable WooCommerce Variable product price range

Are you looking to disable the variable product price range which normally looks like $100-$200. With the snippet of code give below you will be able to hide the price range, and replace it with “From: ” in front of the minimum price. All you need is pasting the following code in your child theme’s functions.php

add_filter( 'woocommerce_variable_price_html', 'variation_price_format_min', 9999, 2 );  
    
function variation_price_format_min( $price, $product ) {  
   $prices = $product->get_variation_prices( true );  
   $min_price = current( $prices['price'] );  
   $price = sprintf( __( 'From: %1$s', 'woocommerce' ), wc_price( $min_price ) );  
   return $price;  
}

## Hide a WooCommerce Category from Search Result

function hide_rentals_from_search_pre_get_posts( $query ) {  
   
 if (!is_admin() && $query->is_main_query() && $query->is_search()) {  
   
 $query->set( ‘post_type’, array( ‘product’ ) );//in the current case i want to hide “rental” category, you can easily replace this with some other product category slug  
 $tax_query = array(  
 array(  
 ‘taxonomy’ => ‘product_cat’,  
 ‘field’ => ‘slug’,  
 ‘terms’ => ‘rentals’,   
 ‘operator’ => ‘NOT IN’,  
 ),  
 );$query->set( ‘tax_query’, $tax_query );  
 }  
   
}add_action( ‘pre_get_posts’, ‘hide_rentals_from_search_pre_get_posts’);

## Remove WooCommerce Checkout fields
```
add_filter( 'woocommerce_checkout_fields' , 'custom_override_checkout_fields' );  
    
function custom_override_checkout_fields( $fields ) {   
    unset($fields['billing']['billing_first_name']);  
    unset($fields['billing']['billing_last_name']);  
    unset($fields['billing']['billing_company']);  
    unset($fields['billing']['billing_address_1']);  
    unset($fields['billing']['billing_address_2']);  
    unset($fields['billing']['billing_city']);  
    unset($fields['billing']['billing_postcode']);  
    unset($fields['billing']['billing_country']);  
    unset($fields['billing']['billing_state']);  
    unset($fields['billing']['billing_phone']);  
    unset($fields['order']['order_comments']);  
    unset($fields['billing']['billing_email']);  
    unset($fields['account']['account_username']);  
    unset($fields['account']['account_password']);  
    unset($fields['account']['account_password-2']);  
    return $fields;  
}

You can customize the above code, let say you want to remove only the address fields then the code will become:

add_filter( 'woocommerce_checkout_fields' , 'custom_override_checkout_fields' );  
    
function custom_override_checkout_fields( $fields ) {   
    unset($fields['billing']['billing_address_1']);  
    unset($fields['billing']['billing_address_2']);  
    unset($fields['billing']['billing_city']);  
    unset($fields['billing']['billing_postcode']);  
    unset($fields['billing']['billing_country']);  
    unset($fields['billing']['billing_state']);  
    return $fields;  
}
```

## Make Woocommerce Shopping Cart responsive
```
Use the code below and include this in your stylesheet:

[@media](http://twitter.com/media) screen and (max-width: 766px) and (min-width: 300px) { /* START Make the cart table responsive */[@media](http://twitter.com/media) screen and (max-width: 600px) { / Force table to not be like tables anymore /  
.woocommerce table.shop_table,  
.woocommerce table.shop_table thead,  
.woocommerce table.shop_table tbody,  
.woocommerce table.shop_table th,  
.woocommerce table.shop_table td,  
.woocommerce table.shop_table tr {  
display: block;  
} / Hide table headers (but not display: none;, for accessibility) /  
.woocommerce table.shop_table thead tr {  
position: absolute;  
top: -9999px;  
left: -9999px;  
} .woocommerce table.shop_table tr {  
/border: 1px solid #d2d3d3; /  
} .woocommerce table.shop_table td {  
/ Behave like a “row” /  
border: 1px solid #d2d3d3;  
position: relative;  
padding-left: 50% !important;  
} .woocommerce table.shop_table {  
border: none;  
} .woocommerce table.shop_table td.product-spacer {  
border-color: #FFF;  
height: 10px;  
} .woocommerce table.shop_table td:before {  
/ Now like a table header /  
position: absolute;  
/ Top/left values mimic padding /  
top: 6px;  
left: 6px;  
width: 25%;  
padding-right: 10px;  
white-space: nowrap;  
} /  
Label the data /  
.woocommerce table.shop_table td.product-remove:before {  
content: “DELETE”;  
} .woocommerce table.shop_table td.product-thumbnail:before {  
content: “IMAGE”;  
} .woocommerce table.shop_table td.product-name:before {  
content: “PRODUCT”;  
} .woocommerce table.shop_table td.product-price:before {  
content: “PRICE”;  
} .woocommerce table.shop_table td.product-quantity:before {  
content: “QUANTITY”;  
} .woocommerce table.shop_table td.product-subtotal:before {  
content: “SUBTOTAL”;  
} .woocommerce table.shop_table td.product-total:before {  
content: “TOTAL”;  
} .woocommerce .quantity,  
.woocommerce #content .quantity,  
.woocommerce .quantity,  
.woocommerce #content .quantity {  
margin: 0;  
} .woocommerce table.cart td.actions,  
.woocommerce #content table.cart td.actions {  
text-align: left;  
border:0;  
padding-left: 0 !important;  
} .woocommerce table.cart td.actions .button.alt,  
.woocommerce #content table.cart td.actions .button.alt {  
float: left;  
margin-top: 10px;  
} .woocommerce table.cart td.actions div,  
.woocommerce #content table.cart td.actions div,  
.woocommerce table.cart td.actions input,  
.woocommerce #content table.cart td.actions input {  
margin-bottom: 10px;  
} .woocommerce .cart-collaterals .cart_totals {  
float: left;  
width: 100%;  
text-align: left;  
} .woocommerce .cart-collaterals .cart_totals th,  
.woocommerce .cart-collaterals .cart_totals td {  
border:0 !important;  
} .woocommerce .cart-collaterals .cart_totals table tr.cart-subtotal td,  
.woocommerce .cart-collaterals .cart_totals table tr.shipping td,  
.woocommerce .cart-collaterals .cart_totals table tr.total td {  
padding-left: 6px !important;  
} .woocommerce table.shop_table tr.cart-subtotal td,  
.woocommerce table.shop_table tr.shipping td,  
.woocommerce table.shop_table tr.total td,  
.woocommerce table.shop_table.order_details tfoot th,  
.woocommerce table.shop_table.order_details tfoot td {  
padding-left: 6px !important;  
border:0 !important;  
} .woocommerce table.shop_table tbody {  
padding-top: 10px;  
} .woocommerce .col2-set .col-1,  
.woocommerce .col2-set .col-1,  
.woocommerce .col2-set .col-2,  
.woocommerce .col2-set .col-2,  
.woocommerce form .form-row-first,  
.woocommerce form .form-row-last,  
.woocommerce form .form-row-first,  
.woocommerce form .form-row-last {  
float: none;  
width: 100%;  
} .woocommerce .order_details ul,  
.woocommerce .order_details ul,  
.woocommerce .order_details,  
.woocommerce .order_details {  
padding:0;  
} .woocommerce .order_details li,  
.woocommerce .order_details li {  
clear: left;  
margin-bottom: 10px;  
border:0;  
} / make buttons full width, text wide anyway, improves effectiveness /  
#content table.cart td.actions .button,  
.woocommerce #content table.cart td.actions .input-text,  
.woocommerce #content table.cart td.actions input,  
.woocommerce table.cart td.actions .button,  
.woocommerce table.cart td.actions .input-text,  
.woocommerce table.cart td.actions input,  
.woocommerce #content table.cart td.actions .button,  
.woocommerce #content table.cart td.actions .input-text,  
.woocommerce #content table.cart td.actions input,  
.woocommerce table.cart td.actions .button,  
.woocommerce table.cart td.actions .input-text,  
.woocommerce table.cart td.actions input {  
width: 100%;  
font-size:12px !important;  
} .woocommerce tfoot{  
display:block !important;  
}  
.woocommerce tfoot td{  
width:100% !important;  
display:block !important;  
}  
/ keep coupon at 50% /  
#content table.cart td.actions .coupon .button,  
.woocommerce #content table.cart td.actions .coupon .input-text,  
.woocommerce #content table.cart td.actions .coupon input,  
.woocommerce table.cart td.actions .coupon .button,  
.woocommerce table.cart td.actions .coupon .input-text,  
.woocommerce table.cart td.actions .coupon input,  
.woocommerce #content table.cart td.actions .coupon .button,  
.woocommerce #content table.cart td.actions .coupon .input-text,  
.woocommerce #content table.cart td.actions .coupon input,  
.woocommerce table.cart td.actions .coupon .button,  
.woocommerce table.cart td.actions .coupon .input-text,  
.woocommerce table.cart td.actions .coupon input {  
width: 48%;  
font-size:12px !important;  
} / clean up how coupon inputs display /  
#content table.cart td.actions .coupon,  
.woocommerce table.cart td.actions .coupon,  
.woocommerce #content table.cart td.actions .coupon,  
.woocommerce table.cart td.actions .coupon {  
margin-top: 1.5em;  
} #content table.cart td.actions .coupon .input-text,  
.woocommerce table.cart td.actions .coupon .input-text,  
.woocommerce #content table.cart td.actions .coupon .input-text,  
.woocommerce table.cart td.actions .coupon .input-text {  
margin-bottom: 1em;  
} / remove cross sells, they interfere with flow between cart and cart totals + shipping calculator /  
.woocommerce .cart-collaterals .cross-sells,  
.woocommerce .cart-collaterals .cross-sells {  
display: none;  
} }  
/* END Make the cart table responsive */ }
```
## Check whether user has paid for a product already in WooCommerce
```
function CheckWhetherUserPaid() {$bought = false; // Set HERE ine the array your specific target product IDs$prod_arr = array( '21', '67' ); // Get all customer orders$customer_orders = get_posts( array(  
 'numberposts' => -1,  
 'meta_key' => '_customer_user',  
 'meta_value' => get_current_user_id(),  
 'post_type' => 'shop_order', // WC orders post type  
 'post_status' => 'wc-completed' // Only orders with status “completed”  
));foreach ( $customer_orders as $customer_order ) {// Updated compatibility with WooCommerce 3+  
 $order_id = method_exists( $order, 'get_id' ) ? $order->get_id() : $order->id;  
 $order = wc_get_order( $customer_order ); // Iterating through each current customer products bought in the order  
 foreach ($order->get_items() as $item) {// WC 3+ compatibility  
  if ( version_compare( WC_VERSION, '3.0', '<' ) ) { $product_id = $item['product_id']; } else { $product_id = $item->get_product_id();}// Your condition related to your 2 specific products Ids  
 if ( in_array( $product_id, $prod_arr ) ) {  
  $bought = true;  
 }  
}}// return “true” if one the specifics products have been bought before by customer  
return $bought;}
```
## WooCommerce Holiday/Pause Mode
```
// Trigger Holiday Mode  
add_action ('init', 'woocommerce_holiday_mode');  
   
   
// Disable Cart, Checkout, Add Cart  
function woocommerce_holiday_mode() {  
   remove_action( 'woocommerce_after_shop_loop_item', 'woocommerce_template_loop_add_to_cart', 10 );  
   remove_action( 'woocommerce_single_product_summary', 'woocommerce_template_single_add_to_cart', 30 );  
   remove_action( 'woocommerce_proceed_to_checkout', 'woocommerce_button_proceed_to_checkout', 20 );  
   remove_action( 'woocommerce_checkout_order_review', 'woocommerce_checkout_payment', 20 );  
   add_action( 'woocommerce_before_main_content', 'wc_shop_disabled', 5 );  
   add_action( 'woocommerce_before_cart', 'wc_shop_disabled', 5 );  
   add_action( 'woocommerce_before_checkout_form', 'wc_shop_disabled', 5 );  
}  
   
   
// Show Holiday Notice  
function wc_shop_disabled() {  
        wc_print_notice( 'Our Online Shop is Closed Today :)', 'error');  
}
```
## Deny Checkout if User Has Pending Orders
```
Deny Checkout if User Has Pending Ordersadd_action('woocommerce_after_checkout_validation', 'deny_checkout_user_pending_orders');  
   
function deny_checkout_user_pending_orders( $posted ) {  
   
 global $woocommerce;  
 $checkout_email = $posted['billing_email'];  
 $user = get_user_by( 'email', $checkout_email );  
    
 if ( ! empty( $user ) ) {  
  $customer_orders = get_posts( array(  
    'numberposts' => -1,  
    'meta_key'    => '_customer_user',  
    'meta_value'  => $user->ID,  
    'post_type'   => 'shop_order', // WC orders post type  
    'post_status' => 'wc-pending' // Only orders with status "completed"  
  ) );  
  foreach ( $customer_orders as $customer_order ) {  
    $count++;  
  }  
  if ( $count > 0 ) {  
     wc_add_notice( 'Sorry, please pay your pending orders first by logging into your account', 'error');  
  }  
 }  
   
}
```
## Change Autofocus Field at WooCommerce Checkout

add_filter( 'woocommerce_checkout_fields', 'change_autofocus_checkout_field' );  
   
function change_autofocus_checkout_field( $fields ) {  
 $fields['billing']['billing_first_name']['autofocus'] = false;  
 $fields['billing']['billing_email']['autofocus'] = true;  
 return $fields;  
}

## Show Message After Country Selection @ Checkout
```
// Part 1  
// Add the message notification and place it over the billing section  
// The "display:none" hides it by default  
    
add_action( 'woocommerce_before_checkout_billing_form', 'echo_notice_shipping' );  
    
function echo_notice_shipping() {  
echo '<div class="shipping-notice woocommerce-info" style="display:none">Please allow 5-10 business days for delivery after order processing.</div>';  
}  
    
// Part 2  
// Show or hide message based on billing country  
// The "display:none" hides it by default  
    
add_action( 'woocommerce_after_checkout_form', 'show_notice_shipping' );  
    
function show_notice_shipping(){  
       
    ?>  
    
    <script>  
        jQuery(document).ready(function($){  
    
            // Set the country code (That will display the message)  
            var countryCode = 'FR';  
    
            $('select#billing_country').change(function(){  
    
                selectedCountry = $('select#billing_country').val();  
                    
                if( selectedCountry == countryCode ){  
                    $('.shipping-notice').show();  
                }  
                else {  
                    $('.shipping-notice').hide();  
                }  
            });  
    
        });  
    </script>  
    
    <?php  
       
}
```
## Disable Payment Method for Specific Category
```
add_filter( 'woocommerce_available_payment_gateways', 'unset_gateway_by_category' );  
    
function unset_gateway_by_category( $available_gateways ) {  
    if ( is_admin() ) return $available_gateways;  
    if ( ! is_checkout() ) return $available_gateways;  
    $unset = false;  
    $category_ids = array( 8, 37 ); //change category id here   
    foreach ( WC()->cart->get_cart_contents() as $key => $values ) {  
        $terms = get_the_terms( $values['product_id'], 'product_cat' );      
        foreach ( $terms as $term ) {          
            if ( in_array( $term->term_id, $category_ids ) ) {  
                $unset = true;  
                break;  
            }  
        }  
    }  
    if ( $unset == true ) unset( $available_gateways['cheque'] );  
    return $available_gateways;  
}
```
## Restrict WooCommerce order notes field to a number of characters

add_filter( 'woocommerce_checkout_fields', 'filter_checkout_fields' );   
function filter_checkout_fields( $fields ) {   
   $fields['order']['order_comments']['maxlength'] = 180;   
   return $fields;  
}

## Remove Checkout Terms & Conditions conditionally in WooCommerce

add_action('woocommerce_checkout_init', 'disable_checkout_terms_and_conditions', 10 );  
function disable_checkout_terms_and_conditions(){  
        remove_action( 'woocommerce_checkout_terms_and_conditions', 'wc_checkout_privacy_policy_text', 20 );  
        remove_action( 'woocommerce_checkout_terms_and_conditions', 'wc_terms_and_conditions_page_content', 30 );  
}


### 1 – Add Payment Type to WooCommerce Admin Email

```php
add_action( 'woocommerce_email_after_order_table', 'add_payment_method_to_admin_new_order', 15, 2 );
 
function add_payment_method_to_admin_new_order( $order, $is_admin_email ) {
  if ( $is_admin_email ) {
    echo '<p><strong>Payment Method:</strong> ' . $order->payment_method_title . '</p>';
  }
}
```

### 2 – Up-sells products per page / per line

```php
remove_action( 'woocommerce_after_single_product_summary', 'woocommerce_upsell_display', 15 );
add_action( 'woocommerce_after_single_product_summary', 'woocommerce_output_upsells', 15 );
 
if ( ! function_exists( 'woocommerce_output_upsells' ) ) {
	function woocommerce_output_upsells() {
	    woocommerce_upsell_display( 3,3 ); // Display 3 products in rows of 3
	}
}
```
### 1 – Replace WooCommerce default PayPal logo

```php
/*
 * Replace WooCommerce default PayPal icon
 */
function paypal_checkout_icon() {
 return 'https://www.paypalobjects.com/webstatic/mktg/logo-center/logo_betalen_met_paypal_nl.jpg'; // write your own image URL here
}
add_filter( 'woocommerce_paypal_icon', 'paypal_checkout_icon' );
```

### 2 – Replace default product placeholder image

```php
/*
* goes in theme functions.php or a custom plugin. Replace the image filename/path with your own :)
*
**/
add_action( 'init', 'custom_fix_thumbnail' );
 
function custom_fix_thumbnail() {
  add_filter('woocommerce_placeholder_img_src', 'custom_woocommerce_placeholder_img_src');
   
	function custom_woocommerce_placeholder_img_src( $src ) {
	$upload_dir = wp_upload_dir();
	$uploads = untrailingslashit( $upload_dir['baseurl'] );
	$src = $uploads . '/2012/07/thumb1.jpg';
	 
	return $src;
	}
}
```

### 3 – Remove “Products” from breadcrumb

```php
/*
 * Hide "Products" in WooCommerce breadcrumb
 */
function woo_custom_filter_breadcrumbs_trail ( $trail ) {
  foreach ( $trail as $k => $v ) {
    if ( strtolower( strip_tags( $v ) ) == 'products' ) {
      unset( $trail[$k] );
      break;
    }
  }

  return $trail;
}

add_filter( 'woo_breadcrumbs_trail', 'woo_custom_filter_breadcrumbs_trail', 10 );
```

### 4 – Empty cart

```php
/*
 * Empty WooCommerce cart
 */
function my_empty_cart(){
	global $woocommerce;
	$woocommerce->cart->empty_cart(); 
}
add_action('init', 'my_empty_cart');
```

### 5 – Automatically add product to cart on visit

```php
/*
 * Add item to cart on visit
 */
function add_product_to_cart() {
	if ( ! is_admin() ) {
		global $woocommerce;
		$product_id = 64;
		$found = false;
		//check if product already in cart
		if ( sizeof( $woocommerce->cart->get_cart() ) > 0 ) {
			foreach ( $woocommerce->cart->get_cart() as $cart_item_key => $values ) {
				$_product = $values['data'];
				if ( $_product->id == $product_id )
					$found = true;
			}
			// if product not found, add it
			if ( ! $found )
				$woocommerce->cart->add_to_cart( $product_id );
		} else {
			// if no products in cart, add it
			$woocommerce->cart->add_to_cart( $product_id );
		}
	}
}
add_action( 'init', 'add_product_to_cart' );
```

### 6 – Add a custom currency / symbol

```php
add_filter( 'woocommerce_currencies', 'add_my_currency' );
 
function add_my_currency( $currencies ) {
     $currencies['ABC'] = __( 'Currency name', 'woocommerce' );
     return $currencies;
}
 
add_filter('woocommerce_currency_symbol', 'add_my_currency_symbol', 10, 2);
 
function add_my_currency_symbol( $currency_symbol, $currency ) {
     switch( $currency ) {
          case 'ABC': $currency_symbol = '$'; break;
     }
     return $currency_symbol;
}
```

### 7 – Change add to cart button text

```php
/**
 * Change the add to cart text on single product pages
 */
function woo_custom_cart_button_text() {
	return __('My Button Text', 'woocommerce');
}
add_filter('single_add_to_cart_text', 'woo_custom_cart_button_text');



/**
 * Change the add to cart text on product archives
 */
function woo_archive_custom_cart_button_text() {
	return __( 'My Button Text', 'woocommerce' );
}
add_filter( 'add_to_cart_text', 'woo_archive_custom_cart_button_text' );
```

### 8 – Redirect subscription add to cart to checkout page

```php
/**
 * Redirect subscription add to cart to checkout page
 *
 * @param string $url
 */
function custom_add_to_cart_redirect( $url ) {
  
  $product_id	= (int) $_REQUEST['add-to-cart'];
	if ( class_exists( 'WC_Subscriptions_Product' ) ) {
		if ( WC_Subscriptions_Product::is_subscription( $product_id ) ) {
			return get_permalink(get_option( 'woocommerce_checkout_page_id' ) );
		} else return $url;
	} else return $url;
	
}
add_filter('add_to_cart_redirect', 'custom_add_to_cart_redirect');
```

This snippet requires the Subscriptions plugin.

### 9 – Redirect to checkout page after add to cart

```php
/**
 * Redirect subscription add to cart to checkout page
 *
 * @param none
 */
function add_to_cart_checkout_redirect() {
	wp_safe_redirect( get_permalink( get_option( 'woocommerce_checkout_page_id' ) ) );
	die();
}
add_action( 'woocommerce_add_to_cart',  'add_to_cart_checkout_redirect', 11 );
```

### 10 – CC all emails

```php
 /**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Add another email recipient to all WooCommerce emails
 *
 */
function woo_cc_all_emails() {
  return 'Bcc: youremail@yourdomain.com' . "\r\n";
}
add_filter('woocommerce_email_headers', 'woo_cc_all_emails' );
```

### 11 – Send an email when a new order is completed with coupons used

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Send an email each time an order with coupon(s) is completed
 * The email contains coupon(s) used during checkout process
 *
 */ 
function woo_email_order_coupons( $order_id ) {
        $order = new WC_Order( $order_id );
        
        if( $order->get_used_coupons() ) {
        
          $to = 'youremail@yourcompany.com';
	        $subject = 'New Order Completed';
	        $headers = 'From: My Name ' . "\r\n";
	        
	        $message = 'A new order has been completed.\n';
	        $message .= 'Order ID: '.$order_id.'\n';
	        $message .= 'Coupons used:\n';
	        
	        foreach( $order->get_used_coupons() as $coupon) {
		        $message .= $coupon.'\n';
	        }
	        @wp_mail( $to, $subject, $message, $headers );
        }
}
add_action( 'woocommerce_thankyou', 'woo_email_order_coupons' );
```

### 12 – Change related products number

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Change number of related products on product page
 * Set your own value for 'posts_per_page'
 *
 */ 
function woo_related_products_limit() {
  global $product;
	
	$args = array(
		'post_type'        		=> 'product',
		'no_found_rows'    		=> 1,
		'posts_per_page'   		=> 6,
		'ignore_sticky_posts' 	=> 1,
		'orderby'             	=> $orderby,
		'post__in'            	=> $related,
		'post__not_in'        	=> array($product->id)
	);
	return $args;
}
add_filter( 'woocommerce_related_products_args', 'woo_related_products_limit' );
```

### 13 – Exclude products from a particular category on the shop page

```php
 /**
 * Remove products from shop page by category
 *
 */
function woo_custom_pre_get_posts_query( $q ) {
 
	if ( ! $q->is_main_query() ) return;
	if ( ! $q->is_post_type_archive() ) return;
	
	if ( ! is_admin() && is_shop() ) {
 
		$q->set( 'tax_query', array(array(
			'taxonomy' => 'product_cat',
			'field' => 'slug',
			'terms' => array( 'shoes' ), // Don't display products in the shoes category on the shop page
			'operator' => 'NOT IN'
		)));
	
	}
 
	remove_action( 'pre_get_posts', 'custom_pre_get_posts_query' );
 
}
add_action( 'pre_get_posts', 'woo_custom_pre_get_posts_query' );
```

### 14 – Change shop columns number

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Change product columns number on shop pages
 *
 */
function woo_product_columns_frontend() {
    global $woocommerce;

    // Default Value also used for categories and sub_categories
    $columns = 4;

    // Product List
    if ( is_product_category() ) :
        $columns = 4;
    endif;

    //Related Products
    if ( is_product() ) :
        $columns = 2;
    endif;

    //Cross Sells
    if ( is_checkout() ) :
        $columns = 4;
    endif;

	return $columns;
}
add_filter('loop_shop_columns', 'woo_product_columns_frontend');
```

### 15 – Disable WooCommerce tabs

```php
 /**
 * Remove product tabs
 *
 */
function woo_remove_product_tab($tabs) {

    unset( $tabs['description'] );      		// Remove the description tab
    unset( $tabs['reviews'] ); 					// Remove the reviews tab
    unset( $tabs['additional_information'] );  	// Remove the additional information tab

 	return $tabs;
 
}
add_filter( 'woocommerce_product_tabs', 'woo_remove_product_tab', 98);
```

### 16 – Remove breadcrumb

```php
 /**
 * Remove WooCommerce BreadCrumb
 *
 */
remove_action( 'woocommerce_before_main_content', 'woocommerce_breadcrumb', 20);
```

### 17 – Restrict shipping countries list

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Restrict shipping countries list
 *
 */
function woo_override_checkout_fields( $fields ) { 

	$fields['shipping']['shipping_country'] = array(
		'type'      => 'select',
		'label'     => __('My New Country List', 'woocommerce'),
		'options' 	=> array('AU' => 'Australia')
	);

	return $fields; 
} 
add_filter( 'woocommerce_checkout_fields' , 'woo_override_checkout_fields' );
```

### 18 – Replace “Free!” product string

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Replace "Free!" by a custom string
 *
 */
function woo_my_custom_free_message() {
	return "This product is FREE!";
}

add_filter('woocommerce_free_price_html', 'woo_my_custom_free_message');
```

### 19 – Hide ALL other shipping methods when Free Shipping is available

```php
// Hide ALL shipping options when free shipping is available
add_filter( 'woocommerce_available_shipping_methods', 'hide_all_shipping_when_free_is_available' , 10, 1 );
 
/**
* Hide ALL Shipping option when free shipping is available
*
* @param array $available_methods
*/
function hide_all_shipping_when_free_is_available( $available_methods ) {
 
  	if( isset( $available_methods['free_shipping'] ) ) :
		
		// Get Free Shipping array into a new array
		$freeshipping = array();
		$freeshipping = $available_methods['free_shipping'];
 
		// Empty the $available_methods array
		unset( $available_methods );
 
		// Add Free Shipping back into $avaialble_methods
		$available_methods = array();
		$available_methods[] = $freeshipping;
 
	endif;
 
	return $available_methods;
}
```

### 20 – Make checkout “state” field not required

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Make "state" field not required on checkout
 *
 */
 
add_filter( 'woocommerce_billing_fields', 'woo_filter_state_billing', 10, 1 );
add_filter( 'woocommerce_shipping_fields', 'woo_filter_state_shipping', 10, 1 );

function woo_filter_state_billing( $address_fields ) { 
	$address_fields['billing_state']['required'] = false;
	return $address_fields;
}

function woo_filter_state_shipping( $address_fields ) { 
	$address_fields['shipping_state']['required'] = false;
	return $address_fields;
}
```

### 21 – Create a coupon programatically

```php
$coupon_code = 'UNIQUECODE'; // Code
$amount = '10'; // Amount
$discount_type = 'fixed_cart'; // Type: fixed_cart, percent, fixed_product, percent_product
					
$coupon = array(
	'post_title' => $coupon_code,
	'post_content' => '',
	'post_status' => 'publish',
	'post_author' => 1,
	'post_type'		=> 'shop_coupon'
);
					
$new_coupon_id = wp_insert_post( $coupon );
					
// Add meta
update_post_meta( $new_coupon_id, 'discount_type', $discount_type );
update_post_meta( $new_coupon_id, 'coupon_amount', $amount );
update_post_meta( $new_coupon_id, 'individual_use', 'no' );
update_post_meta( $new_coupon_id, 'product_ids', '' );
update_post_meta( $new_coupon_id, 'exclude_product_ids', '' );
update_post_meta( $new_coupon_id, 'usage_limit', '' );
update_post_meta( $new_coupon_id, 'expiry_date', '' );
update_post_meta( $new_coupon_id, 'apply_before_tax', 'yes' );
update_post_meta( $new_coupon_id, 'free_shipping', 'no' );
```

### 22 – Change email subject lines

```php
/*
 * Subject filters: 
 *   woocommerce_email_subject_new_order
 *   woocommerce_email_subject_customer_procesing_order
 *   woocommerce_email_subject_customer_completed_order
 *   woocommerce_email_subject_customer_invoice
 *   woocommerce_email_subject_customer_note
 *   woocommerce_email_subject_low_stock
 *   woocommerce_email_subject_no_stock
 *   woocommerce_email_subject_backorder
 *   woocommerce_email_subject_customer_new_account
 *   woocommerce_email_subject_customer_invoice_paid
 **/
add_filter('woocommerce_email_subject_new_order', 'change_admin_email_subject', 1, 2);
 
function change_admin_email_subject( $subject, $order ) {
	global $woocommerce;
 
	$blogname = wp_specialchars_decode(get_option('blogname'), ENT_QUOTES);
 
	$subject = sprintf( '[%s] New Customer Order (# %s) from Name %s %s', $blogname, $order->id, $order->billing_first_name, $order->billing_last_name );
 
	return $subject;
}
```

### 23 – Add custom fee to cart

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Add custom fee to cart automatically
 *
 */
function woo_add_cart_fee() {

	global $woocommerce;
	
	if ( is_cart() ) {
		$woocommerce->cart->add_fee( __('Custom', 'woocommerce'), 5 );
	}
	
}
add_action( 'woocommerce_before_cart_table', 'woo_add_cart_fee' );
```

### 24 – Custom added to cart message

```php
/**
 * Custom Add To Cart Messages
 * Add this to your theme functions.php file
 **/
add_filter( 'woocommerce_add_to_cart_message', 'custom_add_to_cart_message' );
function custom_add_to_cart_message() {
	global $woocommerce;
 
	// Output success messages
	if (get_option('woocommerce_cart_redirect_after_add')=='yes') :
 
		$return_to 	= get_permalink(woocommerce_get_page_id('shop'));
 
		$message 	= sprintf('<a href="%s" class="button">%s</a> %s', $return_to, __('Continue Shopping →', 'woocommerce'), __('Product successfully added to your cart.', 'woocommerce') );
 
	else :
 
		$message 	= sprintf('<a href="%s" class="button">%s</a> %s', get_permalink(woocommerce_get_page_id('cart')), __('View Cart →', 'woocommerce'), __('Product successfully added to your cart.', 'woocommerce') );
 
	endif;
 
		return $message;
}
```

### 25 – Add payment method to admin email

```php
/**
 * WooCommerce Extra Feature
 * --------------------------
 *
 * Add payment method to admin new order email
 *
 */
add_action( 'woocommerce_email_after_order_table', 'woo_add_payment_method_to_admin_new_order', 15, 2 ); 

function woo_add_payment_method_to_admin_new_order( $order, $is_admin_email ) { 
	if ( $is_admin_email ) { 
	echo '<p><strong>Payment Method:</strong> ' . $order->payment_method_title . '</p>'; 
	} 
}
```
