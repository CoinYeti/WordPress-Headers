# WordPress-Headers
Manually Clean Wordpress Headers for Load

How to Clean up WordPress Header Section without any Plugin?

Since long time I was using couple of plugins to clean up some of the fields from WordPress Headers. There are quite a few informations which you usually don’t need, I would say in most of the cases.

If you use web version to publish a post on your WordPress site then you should remove all unnecessary links from your WordPress site.

What are advantages?
Page definitely loads faster
Content to Code ratio increase
Your important site contents are now little higher the way search engine reads
Let’s take a look some of the links from WordPress Headers. Below steps will help you cleanup and optimize WordPress header section.

1. Disable XML-RPC RSD link from WordPress Header
WordPress adds EditURI to your site header, which is required if you are publishing post by third party tool.

<link rel="EditURI" type="application/rsd+xml" title="RSD" href="https://crunchify.com/xmlrpc.php?rsd">

How to fix? Add this to your functions.php file:

remove_action ('wp_head', 'rsd_link');

2. Remove WordPress version number
<meta name="generator" content="WordPress 4.9.2">

Below code will remove WordPress generator value from site.

function crunchify_remove_version() {
	return '';
}
add_filter('the_generator', 'crunchify_remove_version');

3. Remove wlwmanifest link
<link rel="wlwmanifest" type="application/wlwmanifest+xml" href="https://cdn.crunchify.com/wp-includes/wlwmanifest.xml">

Code:

remove_action( 'wp_head', 'wlwmanifest_link');

4. Remove shortlink
<link rel="shortlink" href="https://crunchify.com/?p=8112">

Code:

remove_action( 'wp_head', 'wp_shortlink_wp_head');

5. Remove query strings from all static resources


Add below code and all query strings will be stripped out.

function crunchify_cleanup_query_string( $src ){ 
	$parts = explode( '?', $src ); 
	return $parts[0]; 
} 
add_filter( 'script_loader_src', 'crunchify_cleanup_query_string', 15, 1 ); 
add_filter( 'style_loader_src', 'crunchify_cleanup_query_string', 15, 1 );
Note: explore ('?', $src) will remove everything after ? symbol. If you want to remove only query string for with ver then replace ? with ?ver.

6. Remove api.w.org relation link
<link rel="https://api.w.org/" href="https://crunchify.com/wp-json/">

Code:

remove_action('wp_head', 'rest_output_link_wp_head', 10);
remove_action('wp_head', 'wp_oembed_add_discovery_links', 10);
remove_action('template_redirect', 'rest_output_link_header', 11, 0);
Here is a complete code:
Add below code to your theme’s functions.php file and you are all set.

// ******************** Crunchify Tips - Clean up WordPress Header START ********************** //
function crunchify_remove_version() {
	return '';
}
add_filter('the_generator', 'crunchify_remove_version');
 
remove_action('wp_head', 'rest_output_link_wp_head', 10);
remove_action('wp_head', 'wp_oembed_add_discovery_links', 10);
remove_action('template_redirect', 'rest_output_link_header', 11, 0);
 
remove_action ('wp_head', 'rsd_link');
remove_action( 'wp_head', 'wlwmanifest_link');
remove_action( 'wp_head', 'wp_shortlink_wp_head');
 
function crunchify_cleanup_query_string( $src ){ 
	$parts = explode( '?', $src ); 
	return $parts[0]; 
} 
add_filter( 'script_loader_src', 'crunchify_cleanup_query_string', 15, 1 ); 
add_filter( 'style_loader_src', 'crunchify_cleanup_query_string', 15, 1 );
// ******************** Clean up WordPress Header END ********************** //

Enjoy and have a fun.

Now your site has cleaner header section.

Join the Discussion
Share & leave us some comments on what you think about this topic or if you like to add something.
