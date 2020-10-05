# wordpress_help

## Wordpress Developer tools(Post Types, Custom Fields and other developer friendly plugins)
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

## wp_remote_get() and wp_remote_post() and wp_remote_request()

It’s better to execute JSON response like that to keep it managed.

$request = wp_remote_get($url, $options);
 
return load_request($request);
 
function load_request($response) {
  try {
    $json = json_decode( $response['body'] );
  } catch ( Exception $ex ) {
    $json = null;
  }
  return $json;
}


$apiUrl = 'https://example.com/wp-json/wp/v2/posts?page=0&per_page=0';
$response = wp_remote_get($apiUrl);
$responseBody = wp_remote_retrieve_body( $response );
$result = json_decode( $responseBody );
if ( is_array( $result ) && ! is_wp_error( $result ) ) {
    // Work with the $result data
} else {
    // Work with the error
}

```
## [Wordpress Useful Functions](https://github.com/taniarascia/wp-functions/) Read

## Custom API
### Creating a custom endpoint

```sh

<?php
add_action( 'rest_api_init', 'my_first_register_route' );

function my_first_register_route() {
    register_rest_route( 'my-route', 'my-phrase', array(
                    'methods' => 'GET',
                    'callback' => 'custom_phrase',
                )
            );
}

function custom_phrase() {
    return rest_ensure_response( 'My first REST API' );
}
?>

```
#### Restricting access to the endpoint
#### Restricting access can be achieved by using 'permission_callback'.
```sh
<?php
add_action( 'rest_api_init', 'my_register_route' );

function my_register_route() {
    register_rest_route( 'my-route', 'my-phrase', array(
                    'methods' => 'GET',
                    'callback' => 'custom_phrase',
                    'permission_callback' => function() {
                            return current_user_can( 'edit_posts' );
                        },
                )
            );
}

function custom_phrase() {
    return rest_ensure_response( 'Hello World! This is my first REST API' );
}
?>


```

#### Fetching WordPress data using an endpoint

```sh
<?php
add_action( 'rest_api_init', 'my_register_route');

function my_register_route() {
            
        register_rest_route( 'my-route', 'my-posts', array(
                'methods' => 'GET',
                'callback' => 'my_posts',
                'permission_callback' => function() {
                    return current_user_can( 'edit_others_posts' );
                    }, 

        );
}
   
function my_posts() {
            
    // default the author list to all
    $post_author = 'all';

    // get the posts
    $posts_list = get_posts( array( 'type' => 'post', 'author' => $post_author ) );
    $post_data = array();

    foreach( $posts_list as $posts) {
        $post_id = $posts->ID;
        $post_author = $posts->post_author;
        $post_title = $posts->post_title;
        $post_content = $posts->post_content;

        $post_data[ $post_id ][ 'author' ] = $post_author;
        $post_data[ $post_id ][ 'title' ] = $post_title;
        $post_data[ $post_id ][ 'content' ] = $post_content;

    }

    wp_reset_postdata();

    return rest_ensure_response( $post_data );
}
        
?>
```

#### Filtering the data
#### Continuing on the above example, we will now try to fetch posts for a given author only.

```sh
<?php
add_action( 'rest_api_init', 'my_register_route');
function my_register_route() {
            
      register_rest_route( 'my-route', 'my-posts/(?P<id>\d+)', array(
            'methods' => 'GET',
            'callback' => 'my_posts',
            'args' => array(
                    'id' => array( 
                        'validate_callback' => function( $param, $request, $key ) {
                            return is_numeric( $param );
                        }
                    ),
                ),
            'permission_callback' => function() {
                return current_user_can( 'edit_others_posts' );
                }, 
        );
}
   
function my_posts( $data ) {
            
    // default the author list to all
    $post_author = 'all';
    
    // if ID is set
    if( isset( $data[ 'id' ] ) ) {
          $post_author = $data[ 'id' ];
    }
    
    // get the posts
    $posts_list = get_posts( array( 'type' => 'post', 'author' => $post_author ) );
    $post_data = array();
            
    foreach( $posts_list as $posts) {
        $post_id = $posts->ID;
        $post_author = $posts->post_author;
        $post_title = $posts->post_title;
        $post_content = $posts->post_content;
        
        $post_data[ $post_id ][ 'author' ] = $post_author;
        $post_data[ $post_id ][ 'title' ] = $post_title;
        $post_data[ $post_id ][ 'content' ] = $post_content;
    }
            
    wp_reset_postdata();
            
    return rest_ensure_response( $post_data );
}
        
?>
```

```sh
For Replacing Content
// Filter hook
add_filter('the_content',array('hotwebideas_fix_wordpress','fix_spelling'));

class hotwebideas_fix_wordpress
{
	function fix_spelling($content)
	{

		$content= str_replace('header','HEADER',$content);
		$content = $content . "<hr/>Thank you for watching this video tutorial.";
		return "$content";
	}

}

```
```sh

# Insert Post/Pages content, title, publish throgh coding

$args = array (
		'post_title' => 'Your are Starter',
		'post_content' => 'So this is the part of my daily life, it will create my hosala',
		'post_status' => 'publish',
		'post_type' => 'post',
		// 'post_author' => 1

);
//$post_id = wp_insert_post( $args);
echo "the new post id is #". $post_id.'</hr>';
```
