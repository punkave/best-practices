# Email templates

*You can't use any of the stuff you think you can use.*

External stylesheets, style tags, media queries... all don't work even in gmail. Never mind Outlook 2013 which renders almost nothin'.

*Get used to table layout* and inline style attributes.

Your inline style attributes will work in some cases but many will not.

[Here is a depressing list of things you can't do with CSS3.](https://www.campaignmonitor.com/blog/email-marketing/2010/04/css3-support-in-email-clients/) In particular, *box-sizing does not work in gmail, even in a style attribute*.

Of course your image URLs must be absolute. And you must write your content to look as OK as you can if images are not loaded.

For the most portable result use table layout with old school HTML attributes like `bgcolor` rather than CSS. I wish I were joking but this is what works.
