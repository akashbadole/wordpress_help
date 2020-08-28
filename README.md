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

function ra_change_translate_text( $translated_text ) {
	if ( $translated_text == 'Old Text' ) {
		$translated_text = 'New Translation';
	}
	return $translated_text;
}
add_filter( 'gettext', 'ra_change_translate_text', 20 );


## use only one data
function ra_change_translate_text( $translated_text ) {
	if ( $translated_text == 'Old Text' ) {
		$translated_text = 'New Translation';
	}
	return $translated_text;
}
add_filter( 'gettext', 'ra_change_translate_text', 20 );

## use array format to add more text

function akashbadole_change_translate_text_multiple( $translated ) {
	$text = array(
		'Forminator' => 'Survey',
    'Old Text' => 'New Text',
	);
	$translated = str_ireplace(  array_keys($text),  $text,  $translated );
	return $translated;
}
add_filter( 'gettext', 'akashbadole_change_translate_text_multiple', 20 );

