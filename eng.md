# With a little help from pseudo-selectors

> There are plenty of lesser known CSS (pseudo-)selectors available to help you 
write cleaner HTML structure. This not only reduces the number of classes you 
have to use in your HTML, but in some cases also diminish or abolish the logic 
required to determine whether certain element needs a particular class or not 
(i.e. it is the only element, attribute contains certain string etc.). And in 
some cases the latter also means dynamically writing to DOM, which we know is 
bad for performance.

I will point out the ones which seems to me less ubiquitous, yet very useful in
some situations. I will skip out some selectors, like `:first-child()`, `:last-
child()`, `:nth-child()`, `:nth-last-child()`, `:before` and `:after`, which
deserve to be mentioned, but are already quite well known and broadly used.

## `:not()` and `:matches()`

The negation pseudo-class: `:not()` and the matches-any pseudo-class:
`:matches()` are recent additions to the specs, `:matches` not yet well
supported, but `:not()` is.

**Syntax:**

    E:matches(s1, s2)
    /* an E element that matches compound selector s1 and/or compound selector s2 */

I personally think `:not` is among the best additions to the CSS. There are
plenty of use cases, especially in combination with other pseudo-selectors, such
as `:first-child` or `:last-child` (i.e. you want padding/margin for every
element except for the first or last one).

**Syntax:**

    E:not(s1, s2)
    /* an E element that does not match either compound selector s1 or compound selector s2 */

**Example:**

    li:not(:first-child) { }

## `:empty` and `:only-child()`

They are among many structural pseudo-classes, but especially these two seems to
be somehow the lesser known, yet quite useful.

Pseudo class `only-child()` is useful when you want more/less margin/padding if
the the element contains only one child element; or you might decrease font-size
in case of multiple child elements.

**Syntax:**

    E:only-child()
    /* an E element, only child of its parent */

**Example:**

    .items li { margin: 5px 0; font-size: 1.2em; }
    .items li:only-child { margin: 0; font-size: 1.5em; }

The selector is in fact the same as `elem:last-child():first-child()`.

**Syntax:**

    E:empty
    /* an E element that has no children (not even text nodes) */

Keep in mind that even new line is considered a text node, so structure your
HTML accordingly.

**Example:**

    .items-container:empty { background-image: url('./path/to/no-items-bg.png'); }

You can read more about these two and other at [tree-structural pseudo-classes][1].

## `:local-link`

The `:local-link` pseudo-class allows you to style links that points to the same
domain as the current is and differentiate them from external links.

**Syntax:**

    :local-link
    /* matches any link within this domain */
    :local-link(1)
    /* matches any link within this domain until the first slash (ie: galjot.si/) */
    :local-link(2)
    /* the same as above, but it goes until the second slash */
    :local-link(n)
    /* the same as above, but it goes until the nth slash */

This allows you to get rid of an extra class in your HTML if you want to style
your internal links.

Apart from `:local-link`, there are other well known location pseudo classes, as
`:link` **(not yet visited)** and `:visited` **(target which user has already
visited)**, that I would like to see more frequently used, especially on
content-based websites where they come handy, but there are rather some security
considerations about this [issue][2] regarding attacks on a user's history
through CSS `:visited` selectors.

> For privacy reasons, browsers strictly limit the styles you can apply using an 
element selected by this pseudo-class (`:visited`): `color`, `background-color`, 
`border-color`, `outline-color`, `column-rule-color`, `fill` and `stroke`. 
Source: [Privacy and the `:visited` selector][3].

But if you think that's **#bada55**, you should check the following (via
[@krofdrakula][4]):

<blockquote class="twitter-tweet"><p><a href="https://twitter.com/sedovsek">@sedovsek</a> Read this and weep: <a href="http://t.co/OpfS3t4G0G">http://t.co/OpfS3t4G0G</a></p>&mdash; Klemen Slavič (@krofdrakula) <a href="https://twitter.com/krofdrakula/statuses/368444706557947905">August 16, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## User interface related states pseudo-classes

The following pseudo-selectors consist of two categories, [input pseudo-
classes][5] and [user action pseudo-classes][6].

* `:enabled` and `:disabled`
* `:checked`
* `:default`
* `:indeterminate`
* `:read-only` and `:read-write`
* `:valid` and `:invalid`
* `:in-range` and `:out-of-range`
* `:required` and `:optional`
* `:user-error`
* `:active-drop-target`, `:valid-drop-target` and `:invalid-drop-target`

**Examples:**

    input[type="checkbox"]:checked { }
    input:disabled { }

The above are much needed for good **user interface design** of web applications
that require user input. All these are quite self-explanatory, but not all are
yet confirmed and final (some currently in draft), so you might check their
latest status [here][7].

## Substring matching

The following are a part of [attribute selectors][8] and are the only ones
mentioned in this article that are not pseudo-selectos, but are similar in their
behaviour.

**Syntax:**

    E[foo^="bar"]
    /* an E element whose foo attribute value begins exactly with the string "bar" */

**Example:**

    a[href^="http://twitter.com"] { }
    a[href^="mailto:"]

**Syntax:**

    E[foo$="bar"]
    /* an E element whose foo attribute value ends exactly with the string bar */

**Example:**

    a[href$='.pdf'] { }
    a[href$='.doc'] { }
    a[href$='.xls'] { }
    a[href$='.rss'] { }

**Syntax:**

    E[foo*="bar"]
    /* an E element whose foo attribute value contains the substring bar */

**Example:**

    p[class*='post'] { }

## `:first-line`, `:first-letter`

This one differs a bit from the above selectors, but when used subtly, can add a
lot to the content design and add **aesthetic** appeal. These techniques has
been a part of print design for ages, see these [beautiful slides][9]. You
should also take a look at [Jon Tan][10]'s [12 Examples of Paragraph
Typography][11], an article written way back in 2008, solely relying on
selectors to achieve beautiful effects.

You might also want to read [Drop Caps: Historical Use And Current Best
Practices With CSS][12], where [Laura Franz][13] devoted a whole article to drop
caps.

## `:lang`

[Colors might have a different meaning in different cultures][14], so it is now 
possible to change styles accordingly, with `:lang` selector. The `:lang()` 
pseudo-class accepts comma-separated list of one ore more language ranges as its 
argument. 

**Example:**

    html:lang(de)

Learn more about [the language pseudo-class :lang()][15].

There are plenty of more, but I have only pointed out the ones that seems
interesting, yet not so frequently used. If you want to find out more about the
topic, or contribute, visit [Selectors Level 4][16].

[1]: http://www.w3.org/TR/selectors4/#structural-pseudos
[2]: http://dbaron.org/mozilla/visited-privacy
[3]: https://developer.mozilla.org/en-US/docs/Web/CSS/Privacy_and_the_:visited_selector?redirectlocale=en-US&redirectslug=CSS%2FPrivacy_and_the_%3Avisited_selector
[4]: https://twitter.com/krofdrakula
[5]: http://www.w3.org/TR/selectors4/#input-pseudos
[6]: http://www.w3.org/TR/selectors4/#useraction-pseudos
[7]: http://www.w3.org/TR/selectors4/
[8]: http://www.w3.org/TR/css3-selectors/#attribute-selectors
[9]: http://galjot.si/talks/css3-in-your-font-face/#slide12
[10]: https://twitter.com/jontangerine
[11]: http://v1.jontangerine.com/silo/typography/p/
[12]: http://www.smashingmagazine.com/2012/04/03/drop-caps-historical-use-and-current-best-practices/
[13]: https://twitter.com/laura_type
[14]: http://www.informationisbeautiful.net/visualizations/colours-in-cultures/
[15]: http://www.w3.org/TR/selectors4/#the-lang-pseudo
[16]: http://www.w3.org/TR/selectors4/