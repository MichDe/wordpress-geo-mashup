﻿#summary The list of tags, their format, and parameters.
#labels Phase-Deploy

This is the tag reference for Geo Mashup 1.4.x. (Looking for [1.3.x](TagRefOneThree.md)?)



# Geo Mashup Tag Formats #

Tags in post and page content use standard WordPress [Shortcode format](http://codex.wordpress.org/Shortcode_API). Template tags have also been updated to use standard [template tag parameter format](http://codex.wordpress.org/Template_Tags/How_to_Pass_Tag_Parameters) in WordPress templates. These formats are not complicated - the following examples should be all you need to understand them.

## Shortcode Parameters ##

Here's an example of a tag you might put into a post or page:

`[geo_mashup_map height="200" width="400" zoom="2" add_overview_control="false" add_map_type_control="false"]`

The tag is `geo_mashup_map`, after which any number of parameters are specified. Any parameters that are not specified will use a default value from the Geo Mashup Options. If the post or page has a location saved, single default settings are used, otherwise the global defaults are used.

## Template Tag Parameters ##

To get a similar map in a template, use template tag syntax:

`<?php echo GeoMashup::map('height=200&width=400&zoom=2&add_overview_control=false&add_map_type_control=false');?>`

Note the 'echo' before GeoMashup. This is PHP for "display it right here", and as such is commonly used with template tags.

# Tags #

## Category Legend ##

Inserts a table of colored map pins and the categories they go with. Checkboxes will show or hide category markers on the map. See the map tag for ways to customize.

Shortcode Tag: `[geo_mashup_category_legend]`

Template Tag: `<?php echo GeoMashup::category_legend() ?>`

Accepted Parameters:
  * **for\_map** - the name of the map this category legend should work with. Required if the map has a name, otherwise uses the first map on the page.

## Category Name ##

Inserts the name of the currently displayed map category, if any.

Shortcode Tag: `[geo_mashup_category_name]`

Template Tag: `<?php echo GeoMashup::category_name(); ?>`

## Full Post ##

Displays full post content on a global map page.

Shortcode Tag: `[geo_mashup_full_post]`

Template Tag: `<?php echo GeoMashup::full_post(); ?>`

## List Located Posts ##

Inserts a list of all posts with location information.

Shortcode Tag: `[geo_mashup_list_located_posts]`

Template Tag: `<?php echo GeoMashup::list_located_posts(); ?>`

You can use [TagReference#Query\_Variables](TagReference#Query_Variables.md) with this tag.

## List Located Posts By Area ##

Generates a list of located posts broken down by country and administrative area (state/province).
Country heading is only included if there is more than one country.  Names should be in the blog
language if available.

Shortcode Tag: `[geo_mashup_list_located_posts_by_area]`

Template Tag: `<?php echo GeoMashup::list_located_posts_by_area() ?>`

Parameters:
  * **include\_address** - true (omit for false). Includes post address when available.

## Location Info ##

Display information about the location saved for this item (post, page, comment, etc). Blank if the requested information isn't present for a location.

Shortcode Tag: `[geo_mashup_location_info]`

Template Tag: `<?php echo GeoMashup::location_info(); ?>`

Accepted Parameters:

  * **fields** - Possible values: address, geo\_date, lat, lng, locality\_name, admin\_code, country\_code, postal\_code, saved\_name. The fields admin\_name and country\_name will work also, but be aware that this will trigger a GeoNames lookup the first name. Results are cached for subsequent queries. Comma delimited list of fields to print. Default is address.
  * **separator** - Text to print between field values. Default is a comma.
  * **format** - If supplied, used as a format template for output, similar to [PHP's sprintf() format](http://php.net/manual/en/function.sprintf.php).
  * **obect\_name** - Possible values: post, user, comment. If supplied, used with object\_id to display information about an object other than the "current" contextual object.
  * **object\_id** - Integer ID. If supplied, used with object\_name to display information about an object other than the "current" contextual object.

## Map ##

Inserts a map.

When the tag is in a located post or page, Single Maps settings are used by default.
In a post or page without a location, Global Maps settings are used. As a template tag,
this is the behavior inside [The Loop](http://codex.wordpress.org/The_Loop).  In all other locations, Contextual Maps
settings are used.  You can always change the default map content with the map\_content parameter, such as
`[geo_mashup_map map_content="global"]`.

Shortcode Tag: `[geo_mashup_map]`

Template Tag: `<?php echo GeoMashup::map() ?>`

This tag has a lot of accepted parameters, so we'll categorize them.

### Query Variables ###

These affect the objects that are selected to be shown on a global map. A bounding box query on a post map, for example, will select only posts inside the bounding box to appear on the map.

Some fields, like locality\_name and postal\_code, will only work if reverse geocoding is enabled and the target objects have been successfully geocoded.

  * **admin\_code** - Code for an administrative area. In the United States this is the two letter state code.
  * **country\_code** - Two character ISO country code.
  * **limit** - A maximum number of items to map.
  * **locality\_name** - A locality name, usually city or town.
  * **map\_cat** - the ID of the category to display. Accepts a comma separated list of IDs and IDs preceded by a minus sign are excluded, like the [WordPress query\_posts cat parameter](http://codex.wordpress.org/Class_Reference/WP_Query#Category_Parameters).
  * **map\_content** - global, single, or contextual. Overrides the default content of a map.
  * **map\_post\_type** - Possible values: post, page, or custom post types. A comma separated list of post types to include on the map. Default is "any".
  * **minlat** - Smallest decimal latitude for a bounding box query.
  * **maxlat** - Largest decimal latitude for a bounding box query.
  * **minlon** - Smallest decimal longitude for a bounding box query.
  * **maxlon** - Largest decimal longitude for a bounding box query.
  * **near\_lat** - Used with near\_lng and radius\_mi to include objects near a point.
  * **near\_lng** - Used with near\_lat and radius\_mi to include objects near a point.
  * **object\_name** - Possible values: post, user, comment. The type of objects to include on the map.  Default is post, which includes pages as a type of post.
  * **object\_id** - Integer ID of a single object to include identified in combination with object\_name.
  * **object\_ids** - Comma separated list of IDs objects to include identified in combination with object\_name.
  * **postal\_code** - A postal code - ZIP in the US.
  * **radius\_km** - Same as radius\_mi, but takes a value in kilometers instead of miles.
  * **radius\_mi** - Used with near\_lat and near\_lng to include objects near a point.
  * **saved\_name** - The 'Save As' name used for a location.
  * **show\_future** - true, false, or only. Includes future-dated posts. Default is false.
  * **sort** - Possible values: label, object\_id, geo\_date. Order can affect the category legend or listing tags. Default depends on the object\_name parameter.

### Map Options ###

These affect the appearance or behavior of the map.

  * **add\_map\_control** - true or false. Adds zoom/pan controls.
  * **add\_google\_bar** - true or false. Replaces the Google logo with Google's small map search interface. Default is false.
  * **add\_map\_type\_control** - Possible values: G\_NORMAL\_MAP, G\_SATELLITE\_MAP, G\_SATELLITE\_3D\_MAP, G\_HYBRID\_MAP, G\_PHYSICAL\_MAP. Comma separated list of map types to include in the map type control. Not used by default (blank).
  * **add\_overview\_control** - true or false. Adds the overview control.
  * **auto\_info\_open** - true or false. Opens the info window of the most recent or link source post.
  * **auto\_zoom\_max** - auto zoom only up to the specified zoom level. User with zoom="auto".
  * **background\_color** - A [Hex triplet RGB Color](http://en.wikipedia.org/wiki/Web_colors#Hex_triplet) to use for the background before map tiles load. Default is gray, C0C0C0.
  * **center\_lat** - decimal latitude. With center\_lng, an initial center location for the map.
  * **center\_lng** - decimal longitude. With center\_lat, an initial center location for the map.
  * **click\_to\_load** - true or false. Activates the click-to-load feature described above. Default is false.
  * **click\_to\_load\_text** - the text displayed in the click-to-load pane.
  * **cluster\_max\_zoom** - Integer zoom level. The highest level to cluster markers. Not used by default (blank).
  * **enable\_scroll\_wheel\_zoom** - true or false. Enables the use of a mouse scroll wheel to zoom the map.
  * **height** - the height of the map in pixels
  * **load\_empty\_map** - true or false. Loads a map even if the query for markers return no results. Default is false.
  * **load\_kml** - url of a KML file. Displays the KML file on the map.
  * **map\_control** - !GSmallZoomControl, !GSmallMapControl, !GLargeMapControl, !GSmallZoomControl3d, !GLargeMapControl3D
  * **map\_type** - G\_NORMAL\_MAP, G\_SATELLITE\_MAP, G\_HYBRID\_MAP, G\_PHYSICAL\_MAP
  * **marker\_min\_zoom** - hide markers until zoom level, 0 to 20
  * **marker\_select\_info\_window** - true or false. Enables the opening of the info window when a marker is selected. Default is true.
  * **marker\_select\_highlight** - true or false. Enables highlighting of a marker when it is selected. Default is false.
  * **marker\_select\_center** - true or false. Enables centering of a marker when it is selected. Default is false.
  * **marker\_select\_attachments** - true or false. Enables loading of related KML attachments for a marker when it is selected. Default is false.
  * **open\_object\_id** - The ID of an object to select when the map loads. Replaces open\_post\_id in earlier versions.
  * **remove\_geo\_mashup\_logo** - true or false. Removes the Geo Mashup logo from maps (this will be the default in future versions). Default is false.
  * **static** - true or false. Attempts to use the Google Static Maps API to create a fast-loading static image map. Currently limited to the normal map type with one marker color and style.
  * **width** - the width of the map. Use a plain number like 400 for pixels, or include a percentage sign like 85%.
  * **zoom** - auto or valid integer zoom value. Auto will zoom to include all loaded map content. Default is auto.

### Control Parameters ###

These affect the content and behavior of controls that interact with map content, like the category legend.

  * **name** - name for this map. The **for\_map** parameters of other tags is then used to tie them in with correct map in situations where multiple maps are possible.

#### For Use with the Category Legend ####

  * **interactive\_legend** - true or false. Controls the display of checkboxes in map legend. Default is true.
  * **legend\_format** - dl or table. Formats the category legend using a definition list or table. Default is table.

#### For Use with the Tabbed Category Index ####

  * **disable\_tab\_auto\_select** - For use with the tabbed category index, shows all posts to begin with. Allows the info window to remain open.
  * **show\_inactive\_tab\_markers** - Display markers for all tabs, not just the selected one. For use with the tabbed category index.
  * **start\_tab\_category\_id** - Generate tabs for the children of this category id. For use with the tabbed category index.
  * **tab\_index\_group\_size** - Break subcategory lists into groups of this size. For use with the tabbed category index, can be used to make a column layout.

## Post Coordinates ##

This is currently only available as a template tag which you can use to display the coordinates
stored for a page or post. Have a look at [Issue 134](http://code.google.com/p/wordpress-geo-mashup/issues/detail?id=134)
for good examples of usage.

Template Tag: `<?php $coordinates_array = GeoMashup::post_coordinates() ?>`

Accepted Parameters:
  * **places** - number of decimal places to include in coordinates. Default is 10.

Returns:
  * An array containing 'lat' and 'lng' entries, so in the example `$coordinates_array['lat']` would contain latitude.

## Save Location ##

This can be used to set the location for a post when the post editor location search interface is not available.

When a post or page is saved, the location is read from a special shortcode in the content. The
location is saved, and the special shortcode is removed.

For now at least, decimals must be a period, not a comma.

Shortcode Tag: `[geo_mashup_save_location]`

Parameters:
  * **address** - a location description to geocode (address, partial address, place name, etc). Required if used instead of the **lat** and **lng** parameters.
  * **geo\_date** - the date to associate with the location.  A [variety of formats](http://www.gnu.org/software/tar/manual/html_node/Date-input-formats.html) will work. Defaults to current date.
  * **lat** - decimal latitude. Required.
  * **lng** - decimal longitude. Required.
  * **saved name** - if you want the location added to the saved location menu, the name.

## Show on Map Link ##

Inserts a link for the current post or page to overall blog map.

Shortcode Tag: `[geo_mashup_show_on_map_link]`

Template Tag: `<?php echo GeoMashup::show_on_map_link() ?>`

Accepted Parameters:
  * **text** - the text of the link. Default is 'Show on map'.
  * **show\_icon** - true or false, whether to display the geotag icon before the link. Default is true.
  * **zoom** - the zoom level to start at, 0 to 20.

## Tabbed Category Index ##

Generates output that lists located posts by category in markup that can be styled as tabs.
Subcategories are included in their parent category tab under their own heading. Click on post
titles opens the corresponding marker's info window on the map. The map can show all markers, or
just those for the active tab.

Additional CSS is required in the theme stylesheet to get the tab appearance, otherwise the
markup will just look like a series of lists. An example is included in `tab-style-sample.css`.

Shortcode Tag: `[geo_mashup_tabbed_category_index]`

Template Tag: `<?php echo GeoMashup::tabbed_category_index() ?>`

Parameters:
  * **for\_map** - the name of the map this list should work with. Required if the map has a name, otherwise uses the first map on the page.
  * see the map tag for additional options - the map creates the control from the content included on it.

## Visible Posts List ##

Generates a list of post titles that are currently visible on the map.  Clicking the title opens the info window for that post.

Shortcode Tag: `[geo_mashup_visible_posts_list]`

Template Tag: `<?php echo GeoMashup::visible_posts_list() ?>`

Parameters:
  * **for\_map** - the name of the map this list should work with. Required if the map has a name, otherwise uses the first map on the page.

# Help, Comments #

Comments and questions are welcome in [our Google Group](http://groups.google.com/group/wordpress-geo-mashup-plugin). Bugs and feature requests go in the [issue tracker](http://code.google.com/p/wordpress-geo-mashup/issues/list). Search both - your answer may be there!