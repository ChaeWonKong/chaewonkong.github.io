---
layout: post
title: "[TIL] Sitemap.xml"
date: "2020-10-06"
categories:
  - TIL
excerpt: |
  In 'loc' attribute in sitemap.xml, URL must ends with trailing slash, UTF-8 encoded, and properly escaped.
feature_text: |
  ## How to Create a proper sitemap.xml
  In 'loc' attribute in sitemap.xml, URL must ends with trailing slash, UTF-8 encoded, and properly escaped.
feature_image: "https://images.unsplash.com/photo-1479232284091-c8829ec114ad?ixlib=rb-1.2.1&auto=format&fit=crop&w=1351&q=80"
image: "https://images.unsplash.com/photo-1479232284091-c8829ec114ad?ixlib=rb-1.2.1&auto=format&fit=crop&w=1351&q=80"
---

`sitemap.xml` sometimes plays essential role, like SEO(Search Engine Optimization), and implementing Google Adsense.

Let's find out to create a valid sitemap.

## Basics
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://rollingpaper.site/</loc>
  </url>
</urlset>
```

These are the basic form of a sitemap.

- `urlset` encapsulates the file.
- `url` is a parent tag for each URL entity.
- `loc` is the actual URL string.

There are other tags like `lastmod`, `changefreq`...
> [See sitemaps.org - 'XML tag definitions' section](https://www.sitemaps.org/protocol.html)

## Beware
The URL string must be encoded properly.
You can use JavaScript `encodeURI()` to handle this.

Characters like `&`, `'`, `"`, `>`, `<` must be properly escaped.

| Character | Escape Code|
| --- | --- |
Ampersand  & |	&amp;
Single Quote	' |	&apos;
Double Quote	" |	&quot;
Greater Than	> |	&gt;
Less Than	< |	&lt;

The URL string must start with protocol(http, https).
Depends on your web server, URL string must ends with a trailing slash(`/`).

## Referense
- [[Google] Build and submit a sitemap](https://support.google.com/webmasters/answer/183668?hl=en)
- [sitemap.org - protocol](https://www.sitemaps.org/protocol.html)
