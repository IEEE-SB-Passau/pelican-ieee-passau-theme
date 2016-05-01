## IEEE Passau Flatdesign Template
This is a [Pelican](http://getpelican.org) template. Initially this design was created for Wordpress and can be found [here](https://github.com/maxklenk/flatdesign-child-ieee).

It features multi-language usage, a calendar, filled by events, fully searchable content (javascript-based), a mailman-newsletter signup form, submenus for static pages and diverse small additions that were necessary for us.

![alt tag](https://raw.githubusercontent.com/stieglma/pelican-ieee-passau-theme/master/ieee_passau_design.png)

### Setup
This theme uses several plugins, disabling them may work but was up to now not tested.
Necessary plugins are: 
* ```events``` (creates the calendar, *.ics files)
* ```i18n_subsites``` (multi-language plugin)
* ```tipuesearch``` (javascript-based search)
* ```sitemap```
* ```photos``` (gallery plugin)
* ```assets``` (minify css+js)

In order to make these plugins work please ```pip install```:
* ```beautifulsoup4```
* ```icalendar```
* ```pillow```
* ```webassets```
* ```cssmin```

Additionally you have to enable the jinja2 do statements, assets, multilanguage and add some functions to your pelican config files in order to make the theme work properly:

```Python
JINJA_EXTENSIONS = ['jinja2.ext.do', 'webassets.ext.jinja2.AssetsExtions', 'jinja2.ext.i18n']

# path for css files (default values of assets plugin does not work with multilanguage
# so we just specify the css folder of this theme
ASSET_SOURCE_PATHS = ['static/css']

# this method is necessary as the default language is '' isntead of 'de'
# so we need a converter for proper language handling
def getOtherLanguage(path):
    if path.endswith("en"):
        return u'de'
    else:
        return u'en'

def getCurrentLanguage(path):
    return getOtherLanguage(getOtherLanguage))

def toMonthNum(num):
    months = { 1 : 'Jan',
               2 : 'Feb',
               3 : 'Mar',
               4 : 'Apr',
               5 : 'May',
               6 : 'Jun',
               7 : 'Jul',
               8 : 'Aug',
               9 : 'Sep',
              10 : 'Oct',
              11 : 'Nov',
              12 : 'Dec'
             }
    return months[num]

def removeLanguagePrefix(path):
    if path.startswith("../en/") or path.startswith("../de/"):
        return path[6:]
    else:
        return path

JINJA_FILTERS = {
        'getOtherLanguage': getOtherLanguage,
        'getCurrentLanguage': getCurrentLanguage,
        'toMonthNum': toMonthNum,
        'removeLanguagePrefix': removeLanguagePrefix
        }
```

### Customization
We provide several variables that can be used to customize the theme:

```Python
# external links which should be added as menu items
EXTERNAL_LINKS = [
    ('//linktopage', {'de': 'titel', 'en': 'title'})
]

# setting the mail address of the newsletter
MAILINGLIST = 'announce@ieee.students.uni-passau.de'

# where should the sponsors widget occur on the index (blog) page
SPONSOR_WIDGET_PLACE = 2

# header picture settings
LOGO = 'header/ieee-logo.svg'
HEADER_PICTURE = 'header/branch_header.jpg'
DEFAULT_ARTICLE_HEADER = 'header/branch_header.jpg'
DEFAULT_PAGE_HEADER = 'header/branch_header.jpg'

# sponsors settings
SPONSORS_PICTURE_FOLDER = 'sponsors'
SPONSORS = ['a.png', 'b.png', 'c.png']
SPONSORS_PAGE = 'pages/sponsors'

# footer links
FOOTER_LINK_IMPRINT = '//imprint'
FOOTER_LINK_ORGANISATION = '//organisation'
FOOTER_ORGANISATION_NAME = 'Name'
```

For a working configuration you can look [here](https://github.com/stieglma/pelican-ieee-passau/blob/master/pelicanconf.py).
