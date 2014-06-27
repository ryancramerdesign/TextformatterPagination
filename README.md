# Pagination Textformatter

This Textformatter module for ProcessWire enables you to break up a single textarea field 
(using either TinyMCE or CKEditor) into multiple pages. You include all of the content in 
a single rich text field and separate each pagination with a line of hyphens (5+). 
When rendered on the front-end, the user will see pagination links at the bottom of the 
page enabling them to move forward and backward through the content/article. 

Also included is the option for title pagination. That means assigning a title/headline to 
each pagination and giving the user a list of those titles they can click on to move to 
each section of the article. 


## How to install 

In ProcessWire 2.4+ go to your Modules menu in the admin, click the "New" tab and 
type or paste in "TextformatterPagination" and click "Install". 

If you are unable to install using that method, you can also just install the old way 
by copying the files for this module into /site/modules/TextformatterPagination/ and 
clicking "check for new modules" from your Modules screen. 


## How to use

1. In your admin, go to Setup > Fields and edit your Textarea field(s) where you want to
support pagination. On the "Details" tab, select "Pagination" as a Textformatter and save. 
For the sake of examples, we will assume you added it to a field named "body". 

2. You will need enable page number support for any templates where you intend to use this
module. Go to Setup > Templates and edit the template(s) where you will be using 
pagination. On the "URLs" tab, check the box to enable page numbers and Save. For the sake
of examples, we will assume you added it to a template named "basic-page". 

3. Now edit a page where you want to use pagination. It should be a page with a long block
of copy in your "body" field (or whatever field you added it to). Locate the position where 
you want to separate each page and type in a line of at least five hyphens in a row:

> ...This is the last line of text on page 1.
> 
> ----------------
> 
> This is the first line of text on page 2...

The actual markup (HTML) in your editor would look like this: 

`````
<p>This is the last line of text on page 1.</p>
<p>----------------</p>
<p>This is the first line of text on page 2.</p>
`````

Now preview your page to see how it looks. 


### Title pagination

It is common for multi-page articles to have pagination that behaves more as contextual
links at the bottom of the page, like this:

> Next: [Performance and Optimization](#)
> ---------- 
> - [Introduction](#)
> - [Performance and Optimization](#)
> - [Specifications](#)
> - [Conclusion](#)

This Pagination Textformatter will handle all of it for you. Setting up pagination 
titles is simple. Just follow the instructions under "How to use" above, but follow 
each line of 5+ hyphens with the title you want to use. Example:

> ----- Introduction
> 
> This is the first line of text on page 1.
> ...
> This is the last line of text on page 1. 
> 
> ----- Performance and Optimization
> 
> This is the first line of text on page 2. 
> `````

Specifying a title for the first pagination (as in "Introduction" above) is optional.
If a title is not specified at the top of the content, the page title will be used as 
the title for the first pagination. 


## Customization and options

All customization and options are configured in your `/site/config.php` file by specifying
a `$config->TextformatterPagination = array(...);` containing the options you want to 
modify. Below is a list of all options with the default values. There is no need to 
specify all the options. Just specify the ones you want to change from the defaults. Note
that many of these come from the MarkupPagerNav module, which is used by this module
for outputting numbered pagination. 

```````
$config->TextformatterPagination = array(

  /**** TOGGLES *************************************************************/

  // Use numbered pagination?
  'useNumberPagination' => true, 

  // Use title pagination? (when available)
  'useTitlePagination' => true, 

  // Include a "Next" link in title pagination?
  'useTitleNext' => true, 

  // Display title pagination before numbered pagination?
  'titlePaginationFirst' => true, 

  // Throw a Wire404Exception if invalid pagination is accessed?
  'throw404' => true, 


  /**** TITLE PAGINATION MARKUP *********************************************/

  // markup for title pagination
  'titleListMarkup' => "<hr /><ol class='TextformatterPagination'>{out}</ol>", 

  // markup for items in title pagination
  'titleItemMarkup' => "<li><a href='{url}'>{out}</a></li>", 

  // markup for current item in title pagination
  'titleCurrentItemMarkup' => "<li>{out}</li>", 

  // markup for the "Next" link in title pagination
  'titleNextMarkup' => "<p><strong>Next: <a href='{url}'>{out}</a> &raquo;</strong></p>", 


  /**** NUMBERED PAGINATION MARKUP (from MarkupPagerNav) ********************/

  // List container markup. Place {out} where you want the individual items rendered. 
  'listMarkup' => "\n<ul class='MarkupPagerNav'>{out}\n</ul>",

  // List item markup. Place {class} for item class (required), and {out} for item content. 
  'itemMarkup' => "\n\t<li class='{class}'>{out}</li>",

  // Link markup. Place {url} for href attribute, and {out} for label content. 
  'linkMarkup' => "<a href='{url}'><span>{out}</span></a>", 

  // Link markup for current page. Place {url} for href attribute and {out} for label content. 
  'currentLinkMarkup' => "<a href='{url}'><span>{out}</span></a>", 

  // label used for the 'Next' button
  'nextItemLabel' => 'Next', 

  // label used for the 'Previous' button
  'previousItemLabel' => 'Prev', 

  // label used in the seperator item
  'separatorItemLabel' => '&hellip;', 

  // default classes used for list items, according to the type. 
  'separatorItemClass' => 'MarkupPagerNavSeparator', 
  'firstItemClass' => 'MarkupPagerNavFirst', 
  'firstNumberItemClass' => 'MarkupPagerNavFirstNum', 
  'nextItemClass' => 'MarkupPagerNavNext', 
  'previousItemClass' => 'MarkupPagerNavPrevious', 
  'lastItemClass' => 'MarkupPagerNavLast', 
  'lastNumberItemClass' => 'MarkupPagerNavLastNum',
  'currentItemClass' => 'MarkupPagerNavOn', 
  ); 

```````

While it is most common to populate the above from `/site/config.php` it should be 
technically okay to populate it anytime before you need the Textformatter output. For instance,
you could populate the above configuration values from the top of a template file or from 
an `header.inc` or `_init.php` file you might be using in your site. You may find this useful
if any of the configuration options/markup need to change according to the user language, 
for example. 

### Example of configuration for Zurb Foundation pagination

`````````
$config->TextformatterPagination = array(
  'nextItemLabel' => '&raquo;',
  'nextItemClass' => 'arrow',
  'previousItemLabel' => '&laquo;',
  'previousItemClass' => 'arrow',
  'lastItemClass' => 'last',
  'currentItemClass' => 'current',
  'separatorItemLabel' => '&hellip;',
  'separatorItemClass' => 'unavailable',
  'listMarkup' => "<ul class='pagination'>{out}</ul>",
  'itemMarkup' => "<li class='{class}'>{out}</li>",
  'linkMarkup' => "<a href='{url}'>{out}</a>",
  );
`````````

### What about caching?

Because this module uses the built-in ProcessWire page numbers, each of your paginated
URLs is fully cachable by either the built-in template cache or by ProCache. There are not
any significant considerations here other than that you should not disable the `throw404`
configuration option. 

-------
Copyright 2014 by Ryan Cramer
