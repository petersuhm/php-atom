# PHP Atom client

From [RFC 4287](http://tools.ietf.org/html/rfc4287):

> Atom is an XML-based document format that describes lists of related
> information known as "feeds".  Feeds are composed of a number of
> items, known as "entries", each with an extensible set of attached
> metadata.  For example, each entry has a title.

> The primary use case that Atom addresses is the syndication of Web
> content such as weblogs and news headlines to Web sites as well as
> directly to user agents.

Atom example documents, also from
[RFC 4287](http://tools.ietf.org/html/rfc4287):

```xml
<?xml version="1.0" encoding="utf-8"?>
<feed xmlns="http://www.w3.org/2005/Atom">

    <title>Example Feed</title>
    <link href="http://example.org/"/>
    <updated>2003-12-13T18:30:02Z</updated>
    <author>
        <name>John Doe</name>
    </author>
    <id>urn:uuid:60a76c80-d399-11d9-b93C-0003939e0af6</id>

    <entry>
        <title>Atom-Powered Robots Run Amok</title>
        <link href="http://example.org/2003/12/13/atom03"/>
        <id>urn:uuid:1225c695-cfb8-4ebb-aaaa-80da344efa6a</id>
        <updated>2003-12-13T18:30:02Z</updated>
        <summary>Some text.</summary>
    </entry>

</feed>
```
