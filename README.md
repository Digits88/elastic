Roundcube Webmail Skin "Elastic"
================================

This skin package contains a theme for the Roundcube Webmail
software. It can be used, modified and redistributed according to
the terms described in the LICENSE section.

For information about building or modifying Roundcube skins please visit
https://github.com/roundcube/roundcubemail/wiki/Skins

Roboto font from https://google-webfonts-helper.herokuapp.com/fonts/roboto?subsets=cyrillic,latin-ext,cyrillic-ext,latin,greek,greek-ext

LICENSE
-------
The contents of this folder are subject to the Creative Commons
Attribution-ShareAlike License. It is allowed to copy, distribute,
transmit and to adapt the work by keeping credits to the original
authors in the README.md file.
See http://creativecommons.org/licenses/by-sa/3.0/ for details.


PROJECT GOALS
-------------
Create a user interface that is clean and usable with any screen size.
Use new technologies like e.g. flexbox, @media, font-icons or less.
Cleanup css/html and unify as much as possible.


INSTALLATION (development):
---------------------------
1. git clone https://github.com/roundcube/roundcubemail.git
2. cd roundcubemail/skins
3. git clone https://github.com/roundcube/elastic.git
4. Disable all external plugins and set devel_mode=true.


INSTALLATION (production):
--------------------------
All styles are written using LESS syntax. Thus it needs to be compiled
using the `lessc` command line tool. This comes with the `nodejs-less`
RPM package which depends on nodejs.
```
    $ lessc -x styles/styles.less > styles/styles.css
    $ lessc -x styles/print.less > styles/print.css
    $ lessc -x styles/embed.less > styles/embed.css
```
(the -x option minifies the CSS code)

References to image files from the included CSS files can be appended
with cache-buster marks to avoid browser caching issues after updating.

Run `bin/updatecss.sh --dir skins/elastic` from the Roundcube
package before packaging the skin or after installing it on the
destination system.


RULES:
------
- Supported browsers: IE11+, Edge, Last 2 versions for Chrome/Firefox/Safari,
  Android Browser 5+, iOS Safari 7+.

- Minimum supported screen width is 240px (note that even if the device screen
  resolution is e.g.320x372 changing the text size in device settings will reduce
  the resolution)

- Every page (which is not a frame) has following required structure:
```
    <body>
        <div id="layout">
            <div class="menu"></div>
            <div class="sidebar"></div>
            <div class="list"></div>
            <div class="content"></div>
        </div>
    </body>
```
  where `sidebar` and `list` are optional. Which element of the `layout` will be displayed
  as a main view on mobile devices can be defined by adding `selected` class to it.

- The `<html>` element will receive special classes that will be updated on resize
  or orientation change:
    - `touch`: A touch device, screen width <= 1024px,
    - 'layout-large`: Screen width > 1200px,
    - `layout-normal`: Screen width <= 1200px and >= 768px,
    - `layout-small`: Screen width < 768px and > 480px,
    - `layout-phone`: Screen width <= 480px.

  Frames will have the same classes applied as their parent windows.

- Every button, that is not <button> nor <input> should have inner <span class="inner"> element
  for the button label.

- Special attributes:
    - `data-hidden`: Makes a menu entry/button hidden on specified screen sizes.
      Can be used for example for functionality not implemented or that has no sense
      on phones or touch devices. Contains a comma-separated list following values:
      `large` (width > 1200px), `big` (width > 768px), `small` (width =< 768px).

    - `data-content-button`: Makes the action button with this attribute to be copied
      to the content frame header on small/phone screens.

- Special URLs:
    In phone mode we display Prev/Next navigation buttons below the content preview
    frame. We do this e.g. for mail preview or contact preview. Plugins should use
    _action=add* or _action=create* or _nav=hide in the frame URL if the navigation
    should be hidden, which is the case when you create a content object.
