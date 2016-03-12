# argparse-cs

Czech translation of the Python module
[`argparse`](https://docs.python.org/3/library/argparse.html)

## Quick start

0. `msgfmt argparse-cs.po -o locale/cs/LC_MESSAGES/argparse.mo`
0. Follow the integration instructions below

## Instructions

How to extract translation indices from argparse source:
`pygettext --default-domain=argparse /usr/local/lib/python3.5/argparse.py`

How to merge the extracted indices into the existing dictionary:
`msgmerge argparse-cs.po argparse.pot -U`

How to compile the dictionary:
`msgfmt argparse-cs.po -o locale/cs/LC_MESSAGES/argparse.mo`

## How to integrate the Czech translation in your project

All the methods I have found are invasive to some extent.
Choose the one that does not interfere with the requirements of your project.

### Override the global domain of `gettext` (recommended)

Call the following code before any text is output from `argparse`:

```
import gettext

# Set the following variables to suitable values
domain = 'argparse'
localedir = 'locale'

gettext.bindtextdomain(domain, localedir)
gettext.textdomain(domain)
```

The values `domain` and `localedir` must be set so that the .mo dictionary file
is located at `%(localedir)s/cs/LC_MESSAGES/%(domain)s.mo`.

### Override the function `gettext.gettext`

**Before `import argparse`**,
call the following code:

```
import gettext
gettext.gettext = gettext.translation('argparse', localedir='locale', fallback=True).gettext
```

Source: [http://stackoverflow.com/a/22951500/4054250](http://stackoverflow.com/a/22951500/4054250)

### Replace the default dictionary file

Move the .mo dictionary file to `%(default_localedir)s/cs/LC_MESSAGES/%(default_domain)s.mo`,
where `default_domain = gettext.textdomain()`
and `default_localedir = gettext.bindtextdomain(default_domain)`.

