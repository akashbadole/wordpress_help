
<h2><strong>Definition of Terms</strong></h2>
<p>A <strong>Hook</strong> is a generic term in WordPress that refers to places where you can add your own code or change what WordPress is doing or outputting by default. Two types of hooks exist in WordPress: actions and filters.</p>
<p>An <strong>Action</strong> in WordPress is a hook that is triggered at specific time when WordPress is running and lets you take an action. This can include things like creating a widget when WordPress is initializing or sending a Tweet when someone publishes a post.</p>
<p>A <strong>Filter</strong> in WordPress allows you get and modify WordPress data before it is sent to the database or the browser. Some examples of filters would include customizing how excerpts are displayed or adding some custom code to the end of a blog post.</p>
<p>At first, it may be a little confusing to figure out whether something is an action or a filter. The important difference is that when you work with a filter, you are going to receive some piece of data and then, at the end of your function, you have to return that data back. With an action, on the other hand, you are not receiving and modifying data, you are simply given a place in the WordPress runtime where you can execute your code.</p>
<h2><strong>Types of Action and Filter Hooks</strong></h2>
<p>The WordPress Codex has two important pages that can help you orient yourself with what hooks are available in WordPress.</p>
<p><em>NOTE: At the time of writing there is a bit of a transition of documentation on hooks from the <a href="http://codex.wordpress.org/">Codex</a> to the <a href="https://developer.wordpress.org/reference/">Code Reference</a>&nbsp;site. Because of this you may find hooks that are listed on the Codex, but not documented. You should be able to find some documentation for all of the hooks if you search for them in the Code Reference, however, it does not have any current pages like the Codex where all hooks are grouped in one place.</em></p>
<p>The <a href="http://codex.wordpress.org/Plugin_API/Action_Reference">WordPress <strong>Action Reference page</strong></a>&nbsp;has available actions listed by the following categories:</p>
<ul>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Actions_Run_During_a_Typical_Request">Actions Run During a Typical Request</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Actions_Run_During_an_Admin_Page_Request">Actions Run During an Admin Page Request</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Post.2C_Page.2C_Attachment.2C_and_Category_Actions_.28Admin.29">Post, Page, Attachment, and Category Actions (Admin)</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Comment.2C_Ping.2C_and_Trackback_Actions">Comment, Ping, and Trackback Actions</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Blogroll_Actions">Blogroll Actions</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Feed_Actions">Feed Actions</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Template_Actions">Template Actions</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Administrative_Actions">Administrative Actions</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Action_Reference#Dashboard_.22Right_Now.22_Widget_Actions">Dashboard &ldquo;Right Now&rdquo; Widget Actions</a></li>
</ul>
<p>The Codex also has a similar <strong><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference">Filter Reference page</a></strong>, which it lists by the following categories:</p>
<ul>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Post.2C_Page.2C_and_Attachment_.28Upload.29_Filters">Post, Page, and Attachment (Upload) Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Comment.2C_Trackback.2C_and_Ping_Filters">Comment, Trackback, and Ping Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Category_and_Term_Filters">Category and Term Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Link_Filters">Link Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Date_and_Time_Filters">Date and Time Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Author_and_User_Filters">Author and User Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Blogroll_Filters">Blogroll Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Blog_Information_and_Option_Filters">Blog Information and Option Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#General_Text_Filters">General Text Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Administrative_Filters">Administrative Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Rich_Text_Editor_Filters">Rich Text Editor Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Template_Filters">Template Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Registration_.26_Login_Filters">Registration &amp; Login Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Redirect.2FRewrite_Filters">Redirect/Rewrite Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#WP_Query_Filters">WP_Query Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Media_Filters">Media Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Advanced_WordPress_Filters">Advanced WordPress Filters</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Widgets">Widgets</a></li>
<li><a href="http://codex.wordpress.org/Plugin_API/Filter_Reference#Admin_Bar">Admin Bar</a></li>
</ul>
<p>Many of these filter hooks are separated into subcategories: <strong>Database Reads</strong> and <strong>Database Writes</strong>. This depends on whether you are reading from the database prior to displaying on a page or editing a screen, or you are writing code prior to saving data to the database.</p>
<p>Working with hooks in WordPress starts with figuring out what hook you need to tie your code into and then writing the code to modify the data you need or run whatever action you need.</p>
<p>If you get stuck or you are not sure which hook to use, you can usually figure it out by searching for something like: &ldquo;WordPress action for [whatever you want to hook into].&rdquo; &nbsp;The WordPress StackExchange has a number of results with questions like this as well.</p>
<h2><strong>How to Add and Remove Your Own Functions</strong></h2>
<p>If you would like to hook in your own functions, the process is quite simple. You first need to know a few pieces of information. For actions, you&rsquo;ll want to know the name of the hook, as well as when exactly it runs. For filters, you also need to know the name of the hook, but you want to know what value you are going to get and have to return, as well.&nbsp;The final bit of information you need is the name of the function where you have all your code.</p>
<p><strong>How to Hook into an Action</strong></p>
<pre>add_action( $hook, $function_to_add, $priority, $accepted_args );</pre>
<p>The required parameters of the add_action function are the hook and function to add. The priority is an optional integer value&nbsp;based on a scale of 1 to 999 that determines&nbsp;the priority of order for functions tied to that specific hook. Higher priority means it runs later, lower priority means earlier.&nbsp;The last parameter is&nbsp;used less often and it is for when&nbsp;you need to pass or accept multiple arguments.</p>
<p><strong>How to Hook into an&nbsp;Filter</strong></p>
<pre>add_filter( $tag, $function_to_add, $priority, $accepted_args );</pre>
<p>The add_filter works the same way as add_action. You will also have to be careful because sometimes&nbsp;a hook&nbsp;exists as both an action and a filter, or a filter and a function. You will see the real difference with the&nbsp;actual function you call.</p>
<p>Remember that,&nbsp;for a filter, the function_to_add both receives a value and has to return it at the end of the function. Actions, on the other hand, simply run the code they need to and don&rsquo;t return a value.</p>
<p><strong>How to&nbsp;Unhook from Actions and Filters</strong></p>
<p>To remove a hook is quite simple. Use the function remove_action or remove_filter along with the name of the hook, function, and priority. The priority is optional and helpful if you have to unhook a function that is hooked more than once and you only want to remove a specific occurrence of that function.</p>
<pre>remove_action( $tag, $function_to_remove, $priority );</pre>
<pre>remove_filter( $tag, $function_to_remove, $priority );</pre>
<p>Now that we have looked at the basics of how functions are hooked and unhooked, let&rsquo;s take a look at a few real world examples of some different&nbsp;hooks in action.</p>
<h2><strong>Examples WordPress of Hooks in Action</strong></h2>
<p>More than 200 hooks exist in WordPress. Below you will find a few examples of common hooks in use.</p>
<p><strong>Register a Custom Menu in the Admin</strong></p>
<pre>function register_my_custom_menu_page() {
 add_menu_page( 'custom menu title', 'custom menu', 'manage_options', 'myplugin/myplugin-admin.php', '', 'dashicons-admin-site', 6 );
}
add_action( 'admin_menu', 'register_my_custom_menu_page' );</pre>
<p>In the example above&nbsp;you can see&nbsp;the function&nbsp;register_my_custom_menu_page being&nbsp;hooked into the admin_menu action hook. This allows you to&nbsp;run code when the admin menu is being generated. This is most commonly used to add a custom menu link for a&nbsp;plugin or theme.</p>
<p><strong>Change the Excerpt Length</strong></p>
<pre>function excerpt_length_example( $words ) {
 return 15;
}
add_filter( 'excerpt_length', 'excerpt_length_example' );</pre>
<p>In this example, we are using the excerpt_length filter, which provides us with an integer that determines the length used with the_excerpt().&nbsp;If you are unsure about what value is passed to a filter, you can search the WordPress core code for apply_filters( &lsquo;filter_name&rsquo; and&nbsp;look deeper into what is going on with that filter.</p>
<p><strong>Hook into Post Publishing</strong></p>
<pre>function publish_post_tweet($post_ID) {
  global $post;
  // Code to send a tweet with post info
}
add_action('publish_post', 'publish_post_tweet');</pre>
<p>In&nbsp;the pseudo example above, you can see that we are hooking into an action called &ldquo;publish_post&rdquo; which runs when a post is published. You&nbsp;could use this to do something like sending a tweet with information about the published post.</p>
<p>The actual code for this is more complex than we have space to cover, but it serves as a good example of an action you can run&nbsp;when a post is published.</p>
<p><strong>Hook Into&nbsp;Widget Initialization&nbsp;</strong></p>
<pre>function create_my_widget() {
 register_sidebar(array(
 'name' =&gt; __( 'My Sidebar', 'mytheme' ), 
 'id' =&gt; 'my_sidebar',
 'description' =&gt; __( 'The one and only', 'mytheme' ),
 ));
}
add_action( 'widgets_init', 'create_my_widget' );</pre>
<p>Creating a widget is a very simple and common action to add to a theme or plugin. When you do so, you have to hook into the widget_init action.&nbsp;This hook lets you run your code when widgets are being generated within WordPress, so it&rsquo;s the perfect hook to&nbsp;add your own widgets at the same time.</p>
<p><strong>Hook Into&nbsp;Front-end Scripts and Styles</strong></p>
<pre>function theme_styles() {
 wp_enqueue_style( 'bootstrap_css', get_template_directory_uri() . '/css/bootstrap.min.css' );
 wp_enqueue_style( 'main_css', get_template_directory_uri() . '/style.css' );
 wp_enqueue_script( 'bootstrap_js', get_template_directory_uri() . '/js/bootstrap.min.js', array('jquery'), '', true );
 wp_enqueue_script( 'theme_js', get_template_directory_uri() . '/js/theme.js', array('jquery', 'bootstrap_js'), '', true );
}
add_action( 'wp_enqueue_scripts', 'theme_styles' );</pre>
<p>This is a&nbsp;commonly-used hook and one that you&nbsp;learn quite early in WordPress development. It allows you to generate a URL&nbsp;stylesheets and JavaScript files on the front-end of your theme.&nbsp;This is the preferred method of linking to CSS and JS, rather than hardcoding the links in your theme header.</p>
From <a href="https://blog.teamtreehouse.com/hooks-wordpress-actions-filters-examples">here</a>
