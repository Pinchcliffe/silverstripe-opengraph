# Opengraph module for Silverstripe

This module provides a complete implementation of each of the Open Graph types as documented at <http://ogp.me/>

Open Graph object types may be applied to any Page or DataObject by applying the appropriate interface.

For instance, if your page represents a music album you would implement the IOGMusicAlbum interface.

By default, the module will attempt to classify pages as the og:website type, and automatically
generate appropriate meta tags for it. This is all that most websites require to adequately interact
with Facebook.

## Credits and Authors

 * Damian Mooyman - <https://github.com/tractorcow/silverstripe-opengraph>

## License

 * TODO

## Requirements

 * SilverStripe 2.4.5, may work on lower versions or 3.0
 * PHP 5.2

## Installation Instructions

 * Extract all files into the 'opengraph' folder under your Silverstripe root.
 * If you are working with video files, you might want to install <https://github.com/tractorcow/silverstripe-mediadata>
   alongside this to extract video dimension for opengraph tags.

## Implementing Open Graph object properties

To get specific information on each of the fields an opengraph object can have, check
out the various implementations of each in the /interfaces/ObjectTypes folder.

The basic opengraph object has a set of required properties (as defined by IOGObjectRequired)
and additionally a set of optional properties (as defined by IOGObjectExplicity).

Since most of the field values are generated by the page extension class OpenGraphPageExtension
automatically, you don't need to explicitly implement either of these. These should however
should be used as a guide to what can be specified.

All object type interfaces extend the bare IOGObject interface, allowing pages 
which implement them to continue continue delegating the responsibility for generating
these values by the page extension.

## Adding new types

If you wish to add a new og:type you will need to:
 * Create an interface that extends IOGObject that defines the fields (if any)
   that your object will publish
 * Extend the OpenGraphBuilder class and override the BuildTags function
   to generate the actual HTML for the tags in your interface
 * Implement your interface on pages of the new type
 * Register your object type with the following code:

```php
OpenGraph::register_type('type-name', IOGMyObjectInterface, MyObjectTagBuilder);
```

## Adding tags to the default type

You can decorate the OpenGraphBuilder object instead of extending it if you need
to add additional tags to all object types.

The example below shows how to add extra fields from the Page and SiteConfig
to the set of opengraph tags.

```php
Object::add_extension('OpenGraphBuilder', 'OpengraphBuilderExtension');

class OpengraphBuilderExtension extends Extension
{
    function updateApplicationMetaTags(&$tags, $siteconfig)
    {
        $this->owner->AppendTag($tags, 'og:application-name', $siteconfig->Title);
    }
    
    function updateDefaultMetaTags(&$tags, $page)
    {
        $this->owner->AppendTag($tags, 'og:page-menu-name', $page->MenuTitle);
    }
}
```

## Need more help?

Message or email me at damian.mooyman@gmail.com or, well, read the code!

## Apologies

I went a bit crazy with this module! Good old interfaces eh?