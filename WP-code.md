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
## For Replacing Content
```sh

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
# Insert Post/Pages content, title, publish throgh coding
```sh



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
## add extra profile field in wordpress
```sh

function my_show_profile_fields($user){?>

<h3>Social Media Information</h3>
	<table class="form-table">
		<tr>
			<th>
				<label for="jobtitle"><?php _e("Job Title");?></label>
			</th>
			<td>
				<input type="text" name="jobtitle" id="jobtitle" value="<?php echo esc_attr( get_the_author_meta( 'jobtitle',  $user->ID) ); ?>" class="regular-text" >
				<span class="description">Please Enter Job Title</span>
			</td>
		</tr>
	</table>
<!-- the_author_meta( $meta_key, $user_id ); -->

<?php }

function my_save_extra_profile_fields($user_id){

	if(!current_user_can( 'edit_user', $user_id )){
		return false;
	}
update_usermeta( $user_id, 'jobtitle', $_REQUEST['jobtitle']);

}


add_action( 'show_user_profile','my_show_profile_fields' );
add_action( 'edit_user_profile', 'my_show_profile_fields' );

add_action('personal_options_update','my_save_extra_profile_fields');
add_action( 'edit_user_profile_update', 'my_save_extra_profile_fields' );



?>

```
## Admin Message post, pages
```sh

// Admin Message post, pages

add_action('all_admin_notices','admin_message' );

function admin_message(){

if(strpos($_SERVER['REQUEST_URI'], 'post-new')>0 || strpos($_REQUEST['REQUEST_URI'],'post.php')>0 || strpos($_REQUEST['REQUEST_URI'],'page.php')>0){

	echo '<p style="border:1px grey solid;margin-top:50px;padding:20px;">Hello Editor <br/>Please see our privacy policy and term and condition</>';
}

}

```
## Dashboard Widget Create
```sh
add_action('wp_dashboard_setup','my_new_dashboard');
function my_new_dashboard(){
	global $wp_meta_boxs;
	
	wp_add_dashboard_widget( 'custom_help_widget','Time and Work','main_dashboard_data' );
}

function main_dashboard_data(){
	global $current_user;

	$username = $current_user -> user_login;
	echo 'Greeting, <b>'.$username . '</b> . Today is ' . Date('m/d/Y');
}
```
## Footer text change
```sh
add_action('admin_footer_text','footer_change');

function footer_change(){
	echo "Thank you for creating with <a href='#'>Akash Website</a>";
}
```

## Shortcode with attributes(parameters)

```sh

add_shortcode( 'date-and-time', 'time_and_date_added' );

function time_and_date_added($atts){

	$a     = shortcode_atts( array(
		'color'   => '#FF0000',
		'bgcolor' => '#CCC',
	), $atts);
	$color = $a['color'];
	$color = $a['bgcolor'];
	return '<span style="background-color:'.$bgcolor.';color:'.$color.'">'.date('m/d/Y H:i:s'). '</span>' ;
	
}

```


``sh
Useful SQL Queries To Clean Up Your WordPress Database
After years of usage, your WordPress database can contain weird characters, be filled with data you don’t need anymore, and so on. In this article, you will learn about SQL queries to clean up your WordPress database.

Table of Contents  show 
Two things to note: First, any of these queries should be preceded by a backup of your whole database. Secondly, don’t forget to replace the wp_ table prefix by the prefix used on your WordPress website install, otherwise, the queries won’t work.

How to Run SQL Queries on your WordPress Database
Before getting into the examples, let’s take a moment to check out how it is possible to run SQL queries on a WordPress website. You have three possibilities:

Using SSH: If your WordPress hosting allows SSH connections, you can simply connect to your server and run the queries directly into your MySQL database.
Using PHPMyAdmin: Most WordPress hosting packages come with cPanel and PHPMyAdmin, a web interface that allows you to execute SQL queries.
Using a WordPress plugin: Database My Admin is a WordPress plugin that allows you to run any SQL queries against your WordPress database from within your WP dashboard. If you don’t want to manually run queries and just need to optimize your database, Advanced Database Cleaner might be a plugin to consider.
Clean Up Your WordPress Database From Weird Characters
Encoding problems can be really painful. Instead of manually update all of your posts, here is a query that you can run in order to clean your database from weird characters. Your WordPress site will be much more enjoyable to read for your visitors.

UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€œ', '“');
UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€', '”');
UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€™', '’');
UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€˜', '‘');
UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€”', '–');
UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€“', '—');
UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€¢', '-');
UPDATE wp_posts SET post_content = REPLACE(post_content, 'â€¦', '…');

UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€œ', '“');
UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€', '”');
UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€™', '’');
UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€˜', '‘');
UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€”', '–');
UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€“', '—');
UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€¢', '-');
UPDATE wp_comments SET comment_content = REPLACE(comment_content, 'â€¦', '…');
→ Source: https://digwp.com/2011/07/clean-up-weird-characters-in-database

Reset Administrator Password
WordPress security isn’t something to neglect, and passwords should be changed every once in a while to make sure that your WordPress website stays secure.

As user passwords are stored within the database, it is possible to reset them using a simple SQL query. Simply modify the query below by replacing admin_username by the username of which you want to change the password. new_password is the desired updated password.

UPDATE `wp_users` SET `user_pass` = MD5( 'new_password' ) WHERE `wp_users`.`user_login` = "admin_username";
Note the use of MySQL’s MD5 function, which creates an MD5 hash of the specified password. WordPress security standards require that passwords are stored in the database as MD5 hashes.

Update Links to HTTPS
If you recently switched your WordPress website or blog to HTTPS, you need to update hardcoded links within your articles. This is a tedious task if you do it manually, but it will take you less than a minute if you use SQL queries to update all links contained within your content.

Simply update the query below by replacing yoursite.com by your URL, and run it.

UPDATE wp_posts SET post_content = replace(post_content, 'http://yoursite.com', 'https://yoursite.com');
Close Trackbacks On All Posts At Once
Do you use trackbacks and pings? Nowadays, most people seem to find them useless. In order to get rid of them, you can close trackbacks manually, but this will consume a lot of time. Or, of course, you can use a good old SQL query to perform a database cleaning, as shown below:

UPDATE wp_posts SET ping_status = 'closed';
Mass Delete All Spam Comments
Spam is extremely common and if you chose to give your readers the ability to interact on your articles, there’s no doubt that a lot of spam will be received.

Over the years, WordPress has drastically improved the way spam is handled. If spam is detected, it isn’t displayed on your WordPress site straight away, but instead kept in a queue where you can choose whether to approve them or not.

If your spam queue is long, the fastest way to mass delete all spam comments is to run the following SQL query:

DELETE FROM wp_comments WHERE comment_approved = 'spam';
Get Rid Of All Unused Shortcodes
WordPress shortcodes are very useful and make it easy to embed info in your articles without having to modify any of your WordPress themes. Nowadays, a wide array of WordPress plugins offer shortcodes that can be used to integrate data within the WordPress editor.

But unused shortcodes can create readability problems: Once you stop using a shortcode (for example when you switch to another WordPress theme) you’ll find shortcodes in full text within your content. Here’s a SQL query to remove them. Just update the code with the shortcode you want to remove. I’ve used [tweet] in this example.

UPDATE wp_post SET post_content = replace(post_content, '[tweet]', '' ) ;
Delete Specific Post Meta
Post meta are data associated with a specific post. For example, when you create a custom field, the data is stored as meta. It can then be retrieved and displayed on your WordPress website.

If you used to add a specific custom field to your posts but do not need it anymore, you can perform this “database cleaner” query and remove the undesired post meta quickly and effortlessly.

DELETE FROM wp_postmeta WHERE meta_key = 'YourMetaKey';
→ Source: https://www.esoftload.info/10-sql-statements-for-wordpress

Delete All Unused Tags
A decade ago, tags where very popular in blogging. Nowadays, most bloggers and WordPress site owners stopped using them. If you did, save some space on your database by cleaning it from unused tags.

DELETE FROM wp_terms WHERE term_id IN (SELECT term_id FROM wp_term_taxonomy WHERE count = 0 );
DELETE FROM wp_term_taxonomy WHERE term_id not IN (SELECT term_id FROM wp_terms);
DELETE FROM wp_term_relationships WHERE term_taxonomy_id not IN (SELECT term_taxonomy_id FROM wp_term_taxonomy);
Delete Feed Cache
WordPress stores the feed cache in the wp_options table. If you want to flush the feed cache, you can do so by using the following query:

DELETE FROM `wp_options` WHERE `option_name` LIKE ('_transient%_feed_%')
→ Source: https://wpengineer.com/2114/delete-all-feed-cache…

Optimize Your WordPress Database By Removing Transients
WordPress transients are basically a caching feature: They are used to store any kind of data that takes a long time to get, and therefore are returned super fast the next time you need it.

While this is definitely a super useful feature, transients can take a lot of space in your database when left unmanaged, and reduce your WordPress website performance.

To perform an advanced database cleanup, use the query below:

DELETE FROM `wp_options` WHERE `option_name` LIKE ('%\_transient\_%');
It is totally safe to remove WordPress transients from time to time, as WordPress will recreate the needed transients.

Delete All Revisions And Their Metadata
Revisions are a very useful feature, but if you don’t delete the many revisions from time to time your database will quickly become very big. The following query deletes all revisions as well as all the metadata associated with the revisions.

DELETE a,b,c FROM wp_posts a WHERE a.post_type = 'revision' LEFT JOIN wp_term_relationships b ON (a.ID = b.object_id) LEFT JOIN wp_postmeta c ON (a.ID = c.post_id);
→ Source: https://onextrapixel.com/13-useful-wordpress-sql-queries-you-wish-you-knew-earlier/

Batch Delete Old Posts
Sometimes, you might need to delete very old articles that are no longer relevant. The following query will delete any article older than 600 days. This value (defined on line 3) can be replaced by any desired date in days.

If you want to make an even better version of this query, what about mixing it with the previous one in order to remove old posts as well as their metadata?

DELETE FROM `wp_posts`
WHERE `post_type` = 'post'
AND DATEDIFF(NOW(), `post_date`) > 600
Remove Comment Agent
By default, when someone leaves a reply on your blog, WordPress saves the user agent in the database. It can be useful for stats, but for 95% of bloggers, it is just useless. This query will replace the user agent with a blank string, which can reduce your database size if you have lots of replies.

update wp_comments set comment_agent ='' ;
Update Admin Email Address
All WordPress data is stored within database tables, which means that it can very easily be updated using SQL queries.

If you need to update the admin email, here is the query to do it.

UPDATE `wp_users` SET `user_email` = "new_email_address" WHERE `wp_users`.`user_login` = "admin";
Of course, this query can be used to update any email contained within your wp_users table. Simply replace admin by the username of the account you want to change the email address.

Batch Disable All WordPress Plugins
It can happen that a faulty plugin breaks your WordPress site. Even worst, depending on how serious the error is, you can end up being unable to access your wp_admin area where you could have been able to deactivate the WordPress plugin causing an error.

In that case, the best thing to do is to deactivate all the plugins using the following SQL query:

UPDATE wp_options SET option_value = '' WHERE option_name = 'active_plugins';
Most likely, this will solve the error and allow you to log back into your WP dashboard, where you can re-activate plugins one by one and identify which one was causing problems.

Switch WordPress Themes using SQL
WordPress stores your site settings within the wp_options database table. Therefore, your active WordPress theme can be modified using a simple SQL query.

This can be very useful if your active theme has an error which prevents you to access your admin dashboard. The following query will restore WordPress’ Twenty Nineteen as the active theme.

UPDATE wp_options SET option_value = 'twentynineteen' WHERE option_name = 'template' or option_name = 'stylesheet';
Change Author Attribution On All Posts At Once
Do you need to change author attribution on many posts? If yes, you don’t have to do it manually. Here’s a handy query to do the job for you.

The first thing to do is to retrieve the IDs of WordPress users. Once logged into MySQL, use the following SQL query to get a list of users, as well as their IDs:

SELECT ID, display_name FROM wp_users;
Let’s consider that NEW_AUTHOR_ID is the ID of the new author, and OLD_AUTHOR_ID is the old author ID. Run this query to assign a new author to all articles currently assigned to OLD_AUTHOR_ID.

UPDATE wp_posts SET post_author=NEW_AUTHOR_ID WHERE post_author=OLD_AUTHOR_ID;
Once this query has been executed, all posts from the old author now appear to have been written by the new author.

```
