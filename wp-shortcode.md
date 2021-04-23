```
// >> Create Shortcode to Display showcase Post Types
 
function akash_create_shortcode_showcase_post_type(){
 
    $args = array(
                    'post_type'      => 'showcase',
                    'posts_per_page' => '10',
                    'publish_status' => 'published',
                 );
 
    $query = new WP_Query($args);
 
    if($query->have_posts()) :
 
        while($query->have_posts()) :
 
            $query->the_post() ;
                     
        $result .= '<div class="showcase-css-item">';
        $result .= '<div class="showcase-css-poster">' . get_the_post_thumbnail() . '</div>';
        $result .= '<div class="showcase-css-name">' . get_the_title() . '</div>';
        $result .= '<div class="showcase-css-desc">' . get_the_content() . '</div>'; 
        $result .= '</div>';
 
        endwhile;
 
        wp_reset_postdata();
 
    endif;    
 
    return $result;            
}
 
add_shortcode( 'showcase-list', 'akash_create_shortcode_showcase_post_type' ); 
 
// shortcode code ends here


/* CSS Classes to use */
 
.showcase-css--item{
 width: 100%;
 margin:0 auto;    
 clear: both;
 margin-bottom: 20px; 
 overflow: auto;
 border-bottom: #eee thin solid;
 padding-bottom: 20px;  
}
 
.showcase-css--poster{
width: 160px;
float: left;
margin-right: 25px; 
}
 
.showcase-css--poster img{
width: 100%;
height: auto;
}
 
.showcase-css--name{
font-size: 35px;
}
```
