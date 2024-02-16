## System Behavior Specification

### Data sources of termportalen.no
#### Redigeringsapplikasjon/wiki
##### Terminology Data

Fetched at runtime from the database.

##### Lazy Localization of labels

Some labels in the frontend are lazily localized. They are fetched at
runtime from the database (based on _redigeringsapplikasjonen_) instead of
being defined in the frontend codebase.

This applies to the following data:

- Termbase labels
- Conceptual domain labels

The frontend has a fallback mechanism for displaying labels when there
is no match for the current locale. The language fallback order is
itself localized and as follows:

- nb: nb -> nn -> en
- nn: nn -> nb -> en
- en: en -> nb -> nn

If a termbase is not supposed to have a translated English name, the
field should be left empty so that the frontend can handle falling
back to other versions.

##### Conceptual Domain Data

Conceptual domains are maintained in the _redigeringsapplikasjon_ in a
separate namespace:
[DOMENE](https://wiki.terminologi.no/index.php?title=DOMENE:DOMENE){:target="\_blank"}.
They are fetched at runtime by the frontend. A visualization of the
domain hierarchy can be found on the page for the
[topdomain](https://wiki.terminologi.no/index.php?title=DOMENE:Toppdomene){:target="\_blank"}.

Only domains that have associated concept data should be set to
"published". Empty domains can be created, linked, and described, but
should remain unpublished for the time being.

#### Markdown Content form git.app
Longer texts (welcome, about etc.) are prerendered from markdown files
in the repository
[terminology-content](https://git.app.uib.no/spraksamlingane/terminologi/terminologi-content/-/tree/main/web){:target="\_blank"}

This means that the application has to be rebuild for changes to be visible.

See section about nuxt content for more information.

#### Localized Labels as Part of Codebase
JSON-files for the localization of labels are part of the codebase in
the monorepo.

### Data sources of Administrasjonsappen
#### Terminology Data

Fetched at runtime from the database.

#### Markdown Content from git.app
Longer texts (mainly documentation) are prerendered from markdown
files in the repository
[terminology-content](https://git.app.uib.no/spraksamlingane/terminologi/terminologi-content/-/tree/main/admin){:target="\_blank"}

This means that the application has to be rebuild for changes to be visible.

See section about nuxt content for more information.

#### Unlocalized Labels as Part of Codebase
Labels are part of the codebase in the monorepo.


#### CMS-based
Administrative data is fetched from the CMS-endpoint at runtime.

### Nuxt content and markdown
termportalen.no and the admin app use the nuxt module
[nuxt-content](https://content.nuxt.com/) to handle longer text
content in the markdown format.

Markdown is a lightweight markup language to write plain text
documents that can be converted to html easily.

#### Markdown
Information on the basic markdown syntax can be found on a [markdown
guide page](https://www.markdownguide.org/cheat-sheet/).

#### Special Markup and Available Components
In addition markdown functionality, nuxt-content allows for the use of
vue components in markdown pages

##### AppLinkContent
Inline component for proper typesetting and handling of links (mainly for termportalen.no)

Required properties:
- ``to``: URL that the hyperlink points to
- ``desc``: Description of the link

Use example:
::BlockQuote
```

:AppLinkContent{to="termportalen.no" desc="Termportalen hjemmeside"}

```
::

##### BlockQuote
Block component for block quotes

Use example:
::BlockQuote
```

::BlockQuote
Content
::

```
::
### Public Endpoint Publishing

Public endpoint to be found here: [sparql.ub.uib.no](https://sparql.ub.uib.no){:target="\_blank"}.

Publicly available data is limited by:

- Skiplist
- List of open licenses

See
[documentation](https://git.app.uib.no/spraksamlingane/terminologi/terminologi-meta#update-public-sparql-endpoint){:target="\_blank"}
for more technical information and the [pipeline
config](https://git.app.uib.no/spraksamlingane/terminologi/terminologi-meta/-/pipeline_schedules){:target="\_blank"}
for current settings.
