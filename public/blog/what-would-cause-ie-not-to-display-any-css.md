# What would cause IE not to display any CSS?

During some recent browser testing for a web site I'm building, I was faced with <abbr title="Internet Explorer">IE</abbr> stripping all the CSS out of my site.

It worked in Firefox (both Mac & PC), Safari and Mac IE, but not PC IE.  So what would cause just IE to fail so spectacularly?


<!--more-->

Here's an example of [CSS failing entirely in IE](http://remysharp.com/wp-content/uploads/2006/12/css_break.html).

At the time of writing I can't validate the CSS - because the cause of my problem appears that my [CSS is breaking the validator](http://jigsaw.w3.org/css-validator/validator?uri=http%3A%2F%2Fremysharp.com%2Fwp-content%2Fuploads%2F2006%2F12%2Fcss_break.css&warning=1&profile=css21&usermedium=all) (note that I've tested successfully with a valid CSS file).

The problem boils down to a [null character](http://en.wikipedia.org/wiki/Null_character) sitting in my CSS.  In my original problem it was in the comments in the top of the file causing the entire CSS to fail and work fine in all other browsers.

Firefox does ignore the line with the null character on, so if it was placed before a background colour or font it would ignore that particular line.  

In IE's case, it completely ignores the CSS.

A couple of ways you can spot this:

1. Put the CSS in the head tag of your HTML and use the [W3 validator](http://validator.w3.org/) - this **will** pick up the null character.
2. View it in [Vim](http://www.vim.org/) - if you can.  They will look like ^@.  Just remove them.
3. Use a bit of Perl to spot the bad boy:

`perl -e '$ctr = 0; while(<>){ $ctr++; chomp; if (/\0/) {print "BAD LINE ($ctr): " . $_ . "\n"}}' YOURFILE.CSS`

Remember that other control characters may well have the same effect, but the null character is easy to end up in your CSS if you've pasted something from another app (in my case, I had copied the hex value for a colour from Image Ready CS and pasted in my CSS file).