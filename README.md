# hugo-robots-meta-tags
A template for Hugo that allows for the inclusion of robots meta tags on pages. Not all Hugo themese include the ability to insert and control **robots** meta tags. Nor does it appear that Hugo itself has this capability. So this partial layout template was created.

**Note:** For a more thorough writeup on the functionality of this template, please see this [blog post](https://dereckcurry.com/posts/robots-meta-tags-in-hugo/).

Including a **\<meta name="robots" content="index, follow" /\>** tag in the **\<head\>\</head\>** section of a web page is one method of indicating to some search engines how they should process the page.

Sadly, not all search engines are well behaved and some will ignore the directives in the **robots** meta tag. So if you really want to keep content out of a search engine, you'll need to provide additional protections for content (password protecting a page, etc.) Your best bet though is to never include the content on a web page to begin with. Search engines are really good at discovering and indexing content.

The **robots** meta tag allows the _index/noindex_ and _follow/nofollow_ directives to be set singly. This template includes both for clarity of intent.

Also, this template doesn't allow for the generation of search engine crawler specific meta tags, just the more general **robots** tag.


## The Robots Meta Tag In Hugo

The basic rules for generating and populating the values for the **robots** meta tag are as follows:

1. By default, no **robots** meta tag should be generated.
2. A **robots** meta tag should be generated if site configuration defines values for the tag, or if a page on the site defines values for the **robots** tag in the front matter configuration.
3. By configuring site values for the **robots** meta tag, the tag should be generated for all pages.
4. By configuring values for the **robots** meta tag on a page, then only the tag should be generated only for that page.
5. If a **robots** tag is to be generated, by default the values would be "index, follow". They can be overriden in the site or page configuration settings.
6. Site configuration values take precedence over the default values. Page configuration values take precedence over both the default and site configuration values.

One thing to be aware of, this template requires the **index** and **follow** configuration values to be either **true** or **false**. If for some reason, either the site or page configurations set them to something other than **true** or **false**, the **robots** meta tag will likely not generate.


## Installing The Template File

Every Hugo theme is a bit different, so the instructions below are somewhat generic. You may need to modify them a bit for your specific theme.

Place the **robots.html** partial template file in the the **/layouts/partials/** directory.

```
root
    layouts
        partials
            robots.html
```

Next, you'll need to copy a theme template file to the **/layouts/** directory. The exact subdirectory that you will need to use is dependent upon your themes current directory structure. So most likely you'll need to replicate the approprate structure in the **/layouts/** directory.

The theme layout template file you need to copy is the one that controls the **\<head\>** content for the pages on your site. For my site's theme, that was the **baseof.html** template. So I copied the file to the **/layouts/_default/** directory, which matched the directory structure of my theme.

```
root
    layouts
        _default
            baseof.html
```

Next, modify the copy of **baseof.html** file to include the **robots.html** partial template.

```
<!DOCTYPE html>
<html lang="{{ .Site.LanguageCode }}">
    <head>
        ... Template Head stuff ...

        {{ partial "robots.txt" . }}

        ... More Template Head stuff ...
    </head>
    <body>
        ... Template Body Stuff
    </body>
</html>
```

The next time the site is compiled/generated, the **robots.html** template should be used to include the **robots** meta tag where appropriate. To include the **robots** meta tag, you'll need to set some configuration values in either the site configuration or the page front matter configuration.


## The Site Configuration

The site configuration if optional. If no **params.robots** is defined, then by default no **robots** meta tag will be generated. However, if either the **index** or **follow** parameter is defined though, the **robots** meta tag will be generated for every page on the site.

To include the **robots** meta tag on only select pages of the site, see the next section for the page front matter configuration settings. You do not need to add the *robots* parameters to the site configuration if you only wish to include the **robots** meta tag on some pages.

The site configuration example below is using TOML for the configuration file. Make modifications appropriate to your site depending on the syntax your site configuration uses.

```
[params.robots]
    index = true
    follow = true
```

For the above example site configuration, after compiling/generated the site every page should include **\<meta name="robots" content="index, follow" /\>** in the **\<head\>** section of the web page source.

Set **index = false** if you would rather use **noindex** as the directive in the **content**. Set **follow = false** if you would rather have **nofollow** as the **content** directive.

If either of the **index** or **follow** site configuration robots paremters are not defined, the template default values of **true** will be used for the missing configuration parameter. In other words, you only need to include the parameter if you intend to set it to **false**. However, for clarity you should probably include both settings if you only need to set one to **false**.


## The Page Front Matter Configuration

The page front matter **robots** parameters are also optional, as the code will use the site configuration values if defined, or the default values if no site configuration values are defined. If no site configuration or page front matter configuration parameter values are defined, then the **robots** meta tag will not be generated.

The page front matter example below is using TOML. If you use a different configuration syntax, you'll need to make modifications appropriate for your site.

```
+++
Title = "A Page That Should Not Be Indexed"
[robots]
    index = false
    follow = true
+++
```

After configuration, the next time the site is compiled/generated, the page should include a **robots** meta tag. For the above example page front matter configuration, the page should include **\<meta name="robots" content="noindex, follow" /\>** in the **\<head\>** section of the web page source.

You only need to set the **robots** parameters if you need to generate a specific **robots** meta tag for a page. The page front matter configuration settings will supercede the default and site configuration settings. You only need to define and set the value for the parameter you wish to override. However, if you define one of the **robots** parameters, it would be best to define the other one too for clarity.

Set **index = true** if you would rather use **index** as the directive in the **content**. Set **follow = false** if you would rather have **nofollow** as the **content** directive.


## Conclusion

Although there is no guarantee a search engine will or will not index content on your site, setting the **robots** meta tags should help. Likewise, the **sitemap.xml** file will also assist (see the [hugo-site-map-exclude repository](https://github.com/dereckcurry/hugo-sitemap-exclude) for a template that will allow you exclude certain pages from Hugo generated sitemaps). Likewise, adding a **robots.txt** file to your site with disallow directives should also provide additional clarity to search engine crawlers.

But the best method to protect the content is to never to include the content on a public site in the first place. If the content is there, it is likely to indexed by a search engine at some point.

As always, test the template with your site before deploying to production. Throroughly test if you trying to trying to keep search engines from indexing certain content on your site.

I hope you found this useful.

Dereck