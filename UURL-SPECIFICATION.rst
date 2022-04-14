User URL specification v1.0.X
================================

The User URL core specification defines a versioned and formalized format,
syntax, and rules used to represent and validate `uurl`.

A `uurl` or user URL is an attempt to standardize existing approaches to
reliably identify and locate users on the internet.

A `uurl` is a URL string used to identify and locate a user in a
mostly universal and uniform way across websites, package managers and databases.

Such a user URL is useful to reliably reference the same user
using a simple and expressive syntax and conventions based on familiar URLs.

See https://github.com/package-url/purl-spec for the User URL specification
and <UURL-SPECIFICATION.rst>`_ for known type definitions.

`uurl` stands for **user URL**.

A `uurl` is a URL composed of four components::

    scheme:type/name?qualifiers

Components are separated by a specific character for unambiguous parsing.

The defintion for each components is:

- **scheme**: this is the URL scheme with the constant value of "usr". One of
  the primary reason for this single scheme is to facilitate the future official
  registration of the "usr" scheme for user URLs. Required.
- **type**: the user "type" or "site" such as facebook, twitter, github, wordpress, stackoverflow, linkedin. As well as package managers such as npm, nuget,
  gem, pypi, etc. Required.
- **name**: the user's username, or email. However they are identified by the service. Required.
- **qualifiers**: extra qualifying data for a user such as an email,
  full name, location, website, organization, etc. Optional and type-specific.

Components are designed such that they for a hierarchy from the most significant
on the left to the least significant components on the right.


A `uurl` must NOT contain a URL Authority i.e. there is no support for
`username`, `password`, `host` and `port` components.


Some `uurl` examples
~~~~~~~~~~~~~~~~~~~~

::

    usr:github/octocat?email=octocat%40github.com
    usr:facebook/zuck?fullname=Mark%20Zuckerberg
    usr:twitter/jpinzer?fullname=Julian%20Pinzer&location=Boston,%20MA
    usr:npm/yyx990803?fullname=Evan%20You


A `uurl` is a URL
~~~~~~~~~~~~~~~~~

- A `uurl` is a valid URL and URI that conforms to the URL definitions or
  specifications at:

  - https://tools.ietf.org/html/rfc3986
  - https://en.wikipedia.org/wiki/URL#Syntax
  - https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax
  - https://url.spec.whatwg.org/

- This is a valid URL because it is a locator even though it has no Authority
  URL component: each `type` has a default location when defined.

- The `uurl` components are mapped to these URL components:

  - `uurl` `scheme`: this is a URL `scheme` with a constant value: `usr`
  - `uurl` `type` and `name` components: these are
    collectively mapped to a URL `path`
  - `uurl` `qualifiers`: this maps to a URL `query`
  - In a `uurl` there is no support for a URL Authority (e.g. NO
    `username`, `password`, `host` and `port` components).

- Special URL schemes as defined in https://url.spec.whatwg.org/ such as
  `file://`, `https://`, `http://` and `ftp://` are NOT valid `uurl` types.
  They are valid URL or URI schemes but they are not `uurl`.
  They may be used to reference URLs in separate attributes outside of a `uurl`
  or in a `uurl` qualifier.


Rules for each `uurl` component
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A `uurl` string is an ASCII URL string composed of four components.

Some components are allowed to use other characters beyond ASCII: these
components must then be UTF-8-encoded strings and percent-encoded as defined in
the "Character encoding" section.

The rules for each component are:

- **scheme**:

  - The `scheme` is a constant with the value `usr`
  - Since a `uurl` never contains a URL Authority, its `scheme` must not be
    suffixed with double slash as in `usr://`` and should use instead
    `usr:`. Otherwise this would be an invalid URI per rfc3986 at
    https://tools.ietf.org/html/rfc3986#section-3.3::

        If a URI does not contain an authority component, then the path
        cannot begin with two slash characters ("//").

    It is therefore incorrect to use such '://' scheme suffix as the URL would
    no longer be valid otherwise. In its canonical form, a `uurl` must
    NOT use such '://' `scheme` suffix but only ':' as a `scheme` suffix.
  - `uurl` parsers must accept URLs such as 'usr://' and must ignore the '//'.
  - `uurl` builders must not create invalid URLs with such double slash '//'.
  - The `scheme` is followed by a ':' separator
  - For example these two uurls are strictly equivalent and the first is in
    canonical form. The second `uurl` with a '//' is an acceptable `uurl` but is
    an invalid URI/URL per rfc3986::

            usr:github/octocat
            usr://github/octocat


- **type**:

  - The user `type` is composed only of ASCII letters and numbers, '.', '+'
    and '-' (period, plus, and dash)
  - The `type` cannot start with a number
  - The `type` cannot contains spaces
  - The `type` must NOT be percent-encoded
  - The `type` is case insensitive. The canonical form is lowercase

- **name**:

  - A `name` must be a percent-encoded string

- **qualifiers**:

  - The `qualifiers` string is prefixed by a '?' separator when not empty
  - This '?' is not part of the `qualifiers`
  - This is a query string composed of zero or more `key=value` pairs each
    separated by a '&' ampersand. A `key` and `value` are separated by the equal
    '=' character
  - These '&' are not part of the `key=value` pairs.
  - `key` must be unique within the keys of the `qualifiers` string
  - `value` cannot be an empty string: a `key=value` pair with an empty `value`
    is the same as no key/value at all for this key
  - For each pair of `key` = `value`:

    - The `key` must be composed only of ASCII letters and numbers, '.', '-' and
      '_' (period, dash and underscore)
    - A `key` cannot start with a number
    - A `key` must NOT be percent-encoded
    - A `key` is case insensitive. The canonical form is lowercase
    - A `key` cannot contains spaces
    - A `value` must be a percent-encoded string
    - The '=' separator is neither part of the `key` nor of the `value`


Character encoding
~~~~~~~~~~~~~~~~~~

For clarity and simplicity a `uurl` is always an ASCII string. To ensure that
there is no ambiguity when parsing a `uurl`, separator characters and non-ASCII
characters must be UTF-encoded and then percent-encoded as defined at::

    https://en.wikipedia.org/wiki/Percent-encoding

Use these rules for percent-encoding and decoding `uurl` components:

- the `type` must NOT be encoded and must NOT contain separators

- the '?' and ':' characters must NOT be encoded when used as
  separators. They may need to be encoded elsewhere

- the ':' `scheme` and `type` separator does not need to and must NOT be encoded.
  It is unambiguous unencoded everywhere

- the '/' used as `type`/`name` segments separator
  does not need to and must NOT be percent-encoded. It is unambiguous unencoded
  everywhere

- the '?' `qualifiers` separator must be encoded as `%3F` elsewhere
- the '=' `qualifiers` key/value separator must NOT be encoded

- All non-ASCII characters must be encoded as UTF-8 and then percent-encoded

It is OK to percent-encode `uurl` components otherwise except for the `type`.
Parsers and builders must always percent-decode and percent-encode `uurl`
components and component segments as explained in the "How to parse" and "How to
build" sections.


How to build `uurl` string from its components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Building a `uurl` ASCII string works from left to right, from `type` to
`qualifiers`.

Note: some extra type-specific normalizations are required.
See the "Known types section" for details.

To build a `uurl` string from its components:


- Start a `uurl` string with the "usr:" `scheme` as a lowercase ASCII string

- Append the `type` string  to the `uurl` as a lowercase ASCII string

  - Append '/' to the `uurl`

- For the `name`:

  - Apply type-specific normalization to the `name` if needed
  - UTF-8-encode the `name` if needed in your programming language
  - Append the percent-encoded `name` to the `uurl`

- If the `qualifiers` are not empty and not composed only of key/value pairs
  where the `value` is empty:

  - Append '?' to the `uurl`
  - Build a list from all key/value pair:

    - discard any pair where the `value` is empty.
    - UTF-8-encode each `value` if needed in your programming language
    - If the `key` is `checksums` and this is a list of `checksums` join this
      list with a ',' to create this qualifier `value`
    - create a string by joining the lowercased `key`, the equal '=' sign and
      the percent-encoded `value` to create a qualifier

  - sort this list of qualifier strings lexicographically
  - join this list of qualifier strings with a '&' ampersand
  - Append this string to the `uurl`


How to parse a `uurl` string in its components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Parsing a `uurl` ASCII string into its components works from right to left,
from `qualifiers` to `type`.

Note: some extra type-specific normalizations are required.
See the "Known types section" for details.

To parse a `uurl` string in its components:

- Split the `uurl` once from right on '?'

  - The left side is the `remainder`
  - The right side is the `qualifiers` string
  - Split the `qualifiers` on '&'. Each part is a `key=value` pair
  - For each pair, split the `key=value` once from left on '=':

    - The `key` is the lowercase left side
    - The `value` is the percent-decoded right side
    - UTF-8-decode the `value` if needed in your programming language
    - Discard any key/value pairs where the value is empty
    - If the `key` is `checksums`, split the `value` on ',' to create
      a list of `checksums`

  - This list of key/value is the `qualifiers` object

- Split the `remainder` once from left on ':'

  - The left side lowercased is the `scheme`
  - The right side is the `remainder`

- Split the `remainder` on '/'

  - The left side lowercased is the `type`
  - Percent-decode the right side. This is the `name`
  - UTF-8-decode this `name` if needed in your programming language
  - Apply type-specific normalization to the `name` if needed
  - This is the `name`


Known `uurl` types
~~~~~~~~~~~~~~~~~~~~

There are several known `uurl` user type definitions tracked in the
separate <UURL-TYPES.rst>`_ document.


Known `qualifiers` key/value pairs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Note: Do not abuse `qualifiers`: it can be tempting to use many qualifier
keys but their usage should be limited to the bare minimum for proper user
identification to ensure that a `uurl` stays compact and readable in most cases.

Additional, separate external attributes stored outside of a `uurl` are the
preferred mechanism to convey extra long and optional information.


With this warning, the known `key` and `value` defined here are valid for use in
all user types:

- `email` is the email address for this user, if it isn't the username.

- `fullname` is the full name of the person behind the user account.

- `location` is where the user states that they are in the world.

- `site` is a website that the user might specify. It could be a personal website, or a link to their twitter, linkedin, github, etc.


Tests
~~~~~

To support the language-neutral testing of `uurl` implementations, a test suite
is provided as JSON document named `usr-test-suite-data.json`. This JSON document
contains an array of objects. Each object represents a test with these key/value
pairs some of which may not be normalized:

- **uurl**: a `uurl` string.
- **canonical**: the same `uurl` string in canonical, normalized form
- **type**: the `type` corresponding to this `uurl`.
- **name**: the `name` corresponding to this `uurl`.
- **qualifiers**: the `qualifiers` corresponding to this `uurl` as an object of
  {key: value} qualifier pairs.
- **is_invalid**: a boolean flag set to true if the test should report an
  error

To test `uurl` parsing and building, a tool can use this test suite and for
every listed test object, run these tests:

- parsing the test canonical `uurl` then re-building a `uurl` from these parsed
  components should return the test canonical `uurl`

- parsing the test `uurl` should return the components parsed from the test
  canonical `uurl`

- parsing the test `uurl` then re-building a `uurl` from these parsed components
  should return the test canonical `uurl`

- building a `uurl` from the test components should return the test canonical `uurl`


License
~~~~~~~

This document is licensed under the MIT license
