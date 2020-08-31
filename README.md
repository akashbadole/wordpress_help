# wordpress_help

##Wordpress Developer tools(Post Types, Custom Fields and other developer friendly plugins)
Pods(https://wordpress.org/plugins/pods/)<br>
Toolset Types(https://wordpress.org/plugins/types/)<br>
Advance Custom Fields(https://wordpress.org/plugins/advanced-custom-fields/)<br>
CPT UI(https://wordpress.org/plugins/custom-post-type-ui/)<br>
CMB2(https://wordpress.org/plugins/cmb2/)<br>
Query Monitor(https://wordpress.org/plugins/query-monitor/)<br>
WP Reset(https://wordpress.org/plugins/wp-reset/)<br>
Simple Theme Demo Importer(https://wordpress.org/plugins/simple-theme-demo-importer/)<br>
Widget Importer Exporter(https://wordpress.org/plugins/widget-importer-exporter)<br>
WP Server Health Stats(https://wordpress.org/plugins/wp-server-stats/)<br>
Developer By Automattic(https://wordpress.org/plugins/developer/)<br>
Debug Bar<br>
Debug Bar Console<br>
Debug Bar Cron<br>
Debug Bar Extender<br>
Rewrite Rules Inspector<br>
Log Deprecated Notices<br>
Log Viewer<br>
User Switching<br>
RTL Tester //theme developer<br>
Regenerate Thumbnails<br>
Simply Show IDs<br>
Theme Test Drive<br>
Theme Check<br>


# Text Translate
```sh
function ra_change_translate_text( $translated_text ) {
	if ( $translated_text == 'Old Text' ) {
		$translated_text = 'New Translation';
	}
	return $translated_text;
}
add_filter( 'gettext', 'ra_change_translate_text', 20 );
```

## use only one data
```sh
function ra_change_translate_text( $translated_text ) {
	if ( $translated_text == 'Old Text' ) {
		$translated_text = 'New Translation';
	}
	return $translated_text;
}
add_filter( 'gettext', 'ra_change_translate_text', 20 );
```

## use array format to add more text
```sh
function akashbadole_change_translate_text_multiple( $translated ) {
	$text = array(
		'Forminator' => 'Survey',
    'Old Text' => 'New Text',
	);
	$translated = str_ireplace(  array_keys($text),  $text,  $translated );
	return $translated;
}
add_filter( 'gettext', 'akashbadole_change_translate_text_multiple', 20 );
```

# Coupon List - Cart Page

```sh
add_action('woocommerce_cart_coupon', 'woocommerce_cart_coupon_list');
function woocommerce_cart_coupon_list() {
    $args = array(
	    'posts_per_page'   => 5,
	    'orderby'          => 'title',
	    'order'            => 'desc',
	    'post_type'        => 'shop_coupon',
	    'post_status'      => 'publish',
	);
    
	$coupons = get_posts( $args );

	$coupon_names = array();
	foreach ( $coupons as $coupon ) {
		// Get the name for each coupon post
		$coupon_name = $coupon->post_title;
		array_push( $coupon_names, $coupon_name );
	}
    ?>
    	<div>
    		Choose your coupon code: 
    	 <?php
    	 	foreach ($coupon_names as $key => $value) {
    	 		echo "<span><b>| " . $value . " |</b></span>\r\n";
    	 	}
    	 ?>
		</div>
    <?php
       
}

```

## Admin Menu 

```sh
function register_custom_menu_page() {
    add_menu_page('custom menu title', 'custom menu', 'add_users', 'custompage', '_custom_menu_page', null, 6); 
}
add_action('admin_menu', 'register_custom_menu_page');

function _custom_menu_page(){
   echo "Admin Page Test";  
```

## Wordpress API Call

```sh
$url = 'https://jsonplaceholder.typicode.com/todos';
$arguments = array(
			'method' => 'GET'
);
   /** @var array|WP_Error $response */
$response = wp_remote_get( $url, $arguments );
 
if (is_wp_error( $response )) {
	$error_message = $response->get_error_message();
	echo "Error Hai";
}

echo "<pre>";
var_dump(wp_remote_retrieve_body( $response ));
echo "</pre>";

// echo $obj->Peter;

echo wp_remote_retrieve_body( $response )->userId . "ss";

```
