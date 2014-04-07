# PHP Atom

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

```xml
<?xml version="1.0" encoding="utf-8"?>
<feed xmlns="http://www.w3.org/2005/Atom">
    <title type="text">dive into mark</title>
    <subtitle type="html">
        A &lt;em&gt;lot&lt;/em&gt; of effort went into making this effortless
    </subtitle>
    <updated>2005-07-31T12:29:29Z</updated>
    <id>tag:example.org,2003:3</id>
    <link rel="alternate" type="text/html"
        hreflang="en" href="http://example.org/"/>
    <link rel="self" type="application/atom+xml"
        href="http://example.org/feed.atom"/>
    <rights>Copyright (c) 2003, Mark Pilgrim</rights>
    <generator uri="http://www.example.com/" version="1.0">
        Example Toolkit
    </generator>
    <entry>
        <title>Atom draft-07 snapshot</title>
        <link rel="alternate" type="text/html"
            href="http://example.org/2005/04/02/atom"/>
        <link rel="enclosure" type="audio/mpeg" length="1337"
            href="http://example.org/audio/ph34r_my_podcast.mp3"/>
        <id>tag:example.org,2003:3.2397</id>
        <updated>2005-07-31T12:29:29Z</updated>
        <published>2003-12-13T08:29:29-04:00</published>
        <author>
            <name>Mark Pilgrim</name>
            <uri>http://example.org/</uri>
            <email>f8dy@example.com</email>
        </author>
        <contributor>
            <name>Sam Ruby</name>
        </contributor>
        <contributor>
            <name>Joe Gregorio</name>
        </contributor>
        <content type="xhtml" xml:lang="en"
            xml:base="http://diveintomark.org/">
            <div xmlns="http://www.w3.org/1999/xhtml">
                <p><i>[Update: The Atom draft is finished.]</i></p>
            </div>
        </content>
    </entry>
</feed>
```

## Notes

From [RFC 4287](http://tools.ietf.org/html/rfc4287):

> This specification describes two kinds of Atom Documents: Atom Feed
> Documents and Atom Entry Documents.

Interpreted in PHP code that might look like:

```php
abstract class AtomDocument {}

class AtomFeedDocument extends AtomDocument {}

class AtomEntryDocument extends AtomDocument {}
```

### Misc

* If the "type" attribute is not provided, Atom Processors MUST behave as though
it were present with a value of "text".
* A Date construct is an element whose content MUST conform to the "date-time"
production in [RFC3339]. In addition, an uppercase "T" character MUST be used to
separate date and time, and an uppercase "Z" character MUST be present in the
absence of a numeric time zone offset.

### Business rules

* atom:feed elements MUST contain one or more atom:author elements,
 unless all of the atom:feed element’s child atom:entry elements
 contain at least one atom:author element.
* atom:feed elements MAY contain any number of atom:category
 elements.
* atom:feed elements MAY contain any number of atom:contributor
 elements.
* atom:feed elements MUST NOT contain more than one atom:generator
 element.
* atom:feed elements MUST NOT contain more than one atom:icon
 element.
* atom:feed elements MUST NOT contain more than one atom:logo
 element.
* atom:feed elements MUST contain exactly one atom:id element.
* atom:feed elements SHOULD contain one atom:link element with a rel
 attribute value of "self". This is the preferred URI for
 retrieving Atom Feed Documents representing this Atom feed.
* atom:feed elements MUST NOT contain more than one atom:link
 element with a rel attribute value of "alternate" that has the
 same combination of type and hreflang attribute values.
* atom:feed elements MAY contain additional atom:link elements
 beyond those described above.
* atom:feed elements MUST NOT contain more than one atom:rights
 element.
* atom:feed elements MUST NOT contain more than one atom:subtitle element.
* atom:feed elements MUST contain exactly one atom:title element.
* atom:feed elements MUST contain exactly one atom:updated element.

```php
class AtomFeedDocument extends AtomDocument
{
    /**
     * @var $authors ElementCollection
     */
    protected $authors;

    /**
     * @var $categories ElementCollection
     */
    protected $categories;

    /**
     * @var $contributors ElementCollection
     */
    protected $contributors;

    /**
     * @var $generator AtomGeneratorElement
     */
    protected $generator;

    /**
     * @var $icon AtomIconElement
     */
    protected $icon;

    /**
     * @var $logo AtomLogoElement
     */
    protected $logo;

    /**
     * @var $id AtomIdElement
     */
    protected $id;

    /**
     * @var $links ElementCollection
     */
    protected $links;

    /**
     * @var $rights AtomRightsElement
     */
    protected $rights;

    /**
     * @var $subtitle AtomSubtitleElement
     */
    protected $subtitle;

    /**
     * @var $title AtomTitleElement
     */
    protected $title;

    /**
     * @var $updated AtomUpdatedElement
     */
    protected $updated;

    /**
     * @var $entries ElementCollection
     */
     protected $elements;
}
```

* atom:entry elements MUST contain one or more atom:author elements,
unless the atom:entry contains an atom:source element that
contains an atom:author element or, in an Atom Feed Document, the
atom:feed element contains an atom:author element itself.
* atom:entry elements MAY contain any number of atom:category
elements.
* atom:entry elements MUST NOT contain more than one atom:content
element.
* atom:entry elements MAY contain any number of atom:contributor
elements.
* atom:entry elements MUST contain exactly one atom:id element.
* atom:entry elements that contain no child atom:content element
MUST contain at least one atom:link element with a rel attribute
value of "alternate".
* atom:entry elements MUST NOT contain more than one atom:link
element with a rel attribute value of "alternate" that has the
same combination of type and hreflang attribute values.
* atom:entry elements MAY contain additional atom:link elements
beyond those described above.
* atom:entry elements MUST NOT contain more than one atom:published
element.
* atom:entry elements MUST NOT contain more than one atom:rights
element.
* atom:entry elements MUST NOT contain more than one atom:source
element.
* atom:entry elements MUST contain an atom:summary element in either
of the following cases:
    * the atom:entry contains an atom:content that has a "src"
attribute (and is thus empty).
    * the atom:entry contains content that is encoded in Base64;
i.e., the "type" attribute of atom:content is a MIME media type
[MIMEREG], but is not an XML media type [RFC3023], does not
begin with "text/", and does not end with "/xml" or "+xml".
* atom:entry elements MUST NOT contain more than one atom:summary
element.
* atom:entry elements MUST contain exactly one atom:title element.
* atom:entry elements MUST contain exactly one atom:updated element.
