# tamufeed.js

The versatile, robust JavaScript library for showing
feeds of items on your web page

## What's it good for?

The most common use case is for displaying event-type feeds, from a Texas A&M
calendar, on a web page. And you can use it for displaying news feeds, too.
This app empowers you to customize its design as you like it, so the form of
appearance it takes is up to you.

## Getting Started

To download, click on [tags](/montyaggie/tamufeed.js/tags)
and grab the latest zip e.g.
[0.1.3](/montyaggie/tamufeed.js/archive/0.1.3.zip).
For your first run, we recommend turning on debugging and watching the
F12 console in your browser. (You can turn it off again when you're 
ready to deploy your work.)

## Demo

* [Check it out! live.](http://cllacdn.tamu.edu/calendar/)

## License

Copyright © 2012, Texas A&M University.

This is [open source](http://www.opensource.org/docs/osd) 
and [free software](http://www.gnu.org/philosophy/free-sw.html),
which means that it is not only available for download free of charge, 
but you have access to the source code and may modify and redistribute 
our software subject to certain restrictions;
for details, read the `LICENSE.txt`.

## Files

These are the files included in this distribution
that you must have to get started:

* `tamufeed.js` is the program's code
* `synchronous.html` or `async.html` is HTML
    * and `main.js` if you're going async
* `styles.css` is the CSS

## Dependencies

### Hard Dependencies

1. [Google Feed API](https://developers.google.com/feed/) will continue w/o incompatible changes until 4/20 in 2015
2. [jQuery](http://jquery.com/) or [zepto.js](http://zeptoJS.com/)

### Optional Dependencies

* [Normalize.css](http://necolas.github.com/normalize.css/) is the new reset.css
* [Require.js](http://requirejs.org/) if you want asynchronous JavaScript loading
* [Underscore.js](http://underscorejs.org/) utility-belt library

If you want to retrieve more than ten entries per feed, 
[obtain a Google API key](https://developers.google.com/maps/documentation/javascript/tutorial#api_key)
and pass it as the `key=` URL parameter when you load the `www.google.com/jsapi` script.

## How it works

tamufeed.js works by initializing service connection,
when the document object model is ready.
Then it pulls feeds, models data, sorts entries, and
prints a view into the stage element on the page.

## Feed Types

We retrieve news feeds from Google, recognizing
[microformats](http://microformats.org/), such as calendar items,
in every entry's `content`.

## How to use

There are two ways to integrate tamufeed.js into your web page or CMS theme.

1. Easy (synchronous)
2. High Performance (asynchronous)

If you're integrating it into a very simple or static web page, the easy 
option is great for you.  And, if your CMS does does not yet have
support for asynchronous script loading (like Drupal), or does not really take
advantage of the high performance of an asynchronous loader (like WordPress),
take the easy road.  But, if your web applications use some
client-side logic, you'll benefit from the high performance path.

### Easy Integration

To integrate this software onto a web page (the old synchronous way), 
you basically need to add four things in your HTML.
Let the demo `synchronous.html` be your guide.

1. The configuration script block, which configures parameters like your feed's address. Put it in the &lt;HEAD&gt; of the document.
2. The text/html script elements, which are the output templates for this script. You can put them almost anywhere.
3. The prerequisite jQuery & Google.com/jsapi script tags and then tamufeed.js. Put them in the same sequence near the closing &lt;/BODY&gt; tag at the bottom (but above #4).
4. And finally, copy the bottom-most inline script block in the demo's HTML
to the bottom of your HTML (immediately following the tags from #3).

Here is an example of #1:

    <script>
        //Configuration
        tamufeed = {
            "selector": "#tamufeed"
            ,"fetchEntries": 9
            ,"url": [
              "http://calendar.tamu.edu/anthropology/upcoming/?format=rss"
              ,"http://calendar.tamu.edu/anthropologydeadlines/upcoming/?format=rss"
            ]
        }//tamufeed
    </script>

For #2, the templates can be copied verbatim out of the `synchronous.html` sample.
Make sure they're above the things in the next two steps.

Here is an example of #3:

    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.2/jquery.min.js"></script>
    <script src="https://www.google.com/jsapi?key=YOUR_GOOGLE_API_KEY_HERE"></script>
    <script src="/path/on/your/server/to/tamufeed.js" charset="utf-8"></script>

For #4, make sure that you copy the bottom-most `<script>` block to the bottom
of your own HTML's body. It has four lines. Three lines test that you've got all
scripts loaded, and the last line tells tamufeed to initialize when
[document.ready](http://api.jquery.com/ready/). It looks like this:

    <script>
    if ("undefined"===typeof $) alert("jQuery was not loaded.");
    if ("undefined"===typeof google) alert("Google JS API was not loaded.");
    if ("undefined"===typeof tamufeed) alert("tamufeed.js was not loaded.");
    else $(function() { tamufeed.init(); });
    </script>

When you've got things running, remove your reliance on the files, styles &
graphics hosted on *our* demo [server](http://cllacdn.tamu.edu/html/home.html).
Do not use our bandwidth. Thanks for using **your own web server** for hosting 
your copy of social media icons, JavaScripts, et cetera.

### High Performance integration

Loading JavaScript software asynchronously speeds your page load performance
because it lets scripts load in parallel (instead of serially).
Is async the best practice for high performance?

* Per css-tricks: [blog](http://css-tricks.com/thinking-async/)
* Per [@souders](http://twitter.com/souders): [video](http://radar.oreilly.com/2012/04/velocity-podcast-series-p1.html), [blog](http://www.stevesouders.com/blog/2010/05/07/wpo-web-performance-optimization/)

If you're unfamiliar with asynchronous loaders, you should definitely educate
yourself first. Read the basics of [require.js](http://requirejs.org/),
et. al. Then learn by example: we provide a straight forward, working demo
for you to kick the tires and learn from.  Our `async.html` demo is intuitive,
and easy to follow once you understand at little about
[AMD](http://requirejs.org/docs/why.html).

## Markup

One thing you must add to your HTML somewhere is the stage element where you
want tamufeed to insert its output into, e.g. `<div id="tamufeed"></div>`.

## Configuration

These parameters are set in the `tamufeed` object
(in [JSON](http://www.json.org/) format).

* `url` feed address; this can be one string for your RSS. Or an array of strings, for several.
* `selector` the CSS selector that locates your stage element uniquely
* `sort` sorts entries
    * `"forward"` sorts entries descending, but events ascending
    * `"reverse"` sorts entries ascending time, but events descending
    * `"shuffle"` sorts entries randomly
* `fetchEntries` number of entries to fetch from the service provider for each feed. For calendar feeds, this should be high enough to still have enough events to display after the historical events are set to `display:none`.
* `wantEntries` number of entries you want to show. After this number of entries is exceeded (not counting any historical events), the CSS class `over` is put onto subsequent entries, enabling CSS to `display:none` them.
* `truncatedStringMaxLength` sets number of characters (default 300)
* `minutesBeforeHistorical` sets slack once dtstart has elapsed before event is labelled historical (default 30)
* `debugging` turns on the F12 console debug messages

## Templates

The software allows the integrator to customize the HTML output via templates
that you host in your HTML. You need to provide these templates (as demonstrated
in the example):

* `feedTemplate` governs what feeds will be marked up with
* `entryTemplate` controls the display of entries (of all types)
* `dateTemplate` formats how the date part of `{{time}}` should look
* `propertyTemplate` is how individual properties will be marked up
* `encasedTemplate` is used to encase any HTML value that could possibly be malformed
* `dateBlockTemplate` decides how `{{dateBlock}}` comes out

### Template Variables

Their variables, where the script injects its content,
are always delimited by mustache/handlebar characters like this: 
`{{LookMaImaVariable}}`. Note that they are case sensitive
(i.e. capitalization does matter and must be precisely the same).
The meaning of the variables should be fairly intuitive when you see
how they're used in demo.

### Template Date Variables

The single-letter variables used with dates is an incomplete subset of
[function.date.php](http://www.php.net/manual/en/function.date.php).
If you encounter one that you would like to use that isn't yet implemented,
[just create an issue](/montyaggie/tamufeed.js/issues/new) and we'll add it.
These work should be working fine:

    `{{w}}` day of the week (0-6 for Sun-Sat)
    `{{S}}` English ordinal suffix for day ["st","nd","rd","th"]
    `{{j}}` day of the month (1-31)
    `{{d}}` day of the month (01-31) with leading zeros
    `{{D}}` day of the month e.g. Mon or Tue
    `{{l}}` day of the month e.g. Monday or Tuesday
    `{{n}}` month (1-12)
    `{{m}}` month (01-12)
    `{{M}}` month, short textual representation, three letters
    `{{F}}` month, full textual representation e.g. January
    `{{Y}}` year (4 digits for 4-digit years) 
    `{{i}}` minutes (00-59) with leading zeros
    `{{g}}` hour (0-12)
    `{{a}}` am or pm
    `{{U}}` Seconds since the Unix Epoch (January 1 1970)

## Style

You'll want to write some CSS styles to govern the appearance of your
feed on your page. Feel free to copy liberally from the styles.css, if they
help you.  Firebug or Safari or Chrome's F12 developer tools are quite helpful,
for introspecting the HTML and trying out styles that you can then put into
your own CSS.

## Naming things

When assigning class names on markup elements, 
we conform to these standards.

* For Events, [hCalendar](http://microformats.org/wiki/hcalendar).
* For Persons, [hCard](http://microformats.org/wiki/hcard).
* For Stories, [hAtom](http://microformats.org/wiki/hatom) or [RSS](http://cyber.law.harvard.edu/rss/rss.html).

## For Programmers

* [Google Feeds API's result object structure](https://developers.google.com/feed/v1/jsondevguide#resultJson)
* The Texas A&M University's calendar software on the server side is
[UNL Event Publisher](http://events.unl.edu/) documented at
http://code.google.com/p/unl-event-publisher/wiki/UNL_UCBCN_Frontend
* Recommended naming conventions
  * Events
     * [hCalendar](http://microformats.org/wiki/hcalendar)
     * http://schema.org/Event
  * Persons
      * [hCard](http://microformats.org/wiki/hcard)
      * http://schema.org/Person
  * Stories
      * [Atom syndication format RFC4287](http://tools.ietf.org/html/rfc4287)
      * http://schema.org/Article
  * Books
      * http://schema.org/Book
  * Apps
      * http://schema.org/SoftwareApplication
  * Schools
      * http://schema.org/EducationalOrganization


## TAMU Support

Information on the Texas A&M's calendar can be found online on its
[help page](http://marcomm.tamu.edu/web/calendar/help.html),
on the [Texas A&M Webmaster blog](http://webmaster.tamu.edu/category/calendar/),
through the [email list](http://marcomm.tamu.edu/web/calendar/documentation.html#listserv),
or by contacting the Texas A&M Calendar Team [directly](calendar@tamu.edu).
