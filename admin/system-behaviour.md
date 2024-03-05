## System Behavior Specification

### Data sources of termportalen.no
#### Redigeringsapplikasjon/wiki
**Terminology Data**

Fetched at runtime from the database.

**Lazy Localization of labels**

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

**Conceptual Domain Data**

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

#### News entries from Administrasjonsappen
The news entries are retrieved from the Sanity Studio API at runtime. In order for a news entry to be displayed, it must meet the following conditions:
- It is set to `published` in the Studio.
- It has a date assigned.
- The date is in the past.

Only the three most recent entries that meet these conditions will be shown on the termportalen-front page.

If languages are not specified when the news entry is created in the Studio, a fallback mechanism will be implemented when the entry is displayed on the front page. The order follows the same sequence as for lazy localization.

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

**Special Markup and Available Components**
In addition markdown functionality, nuxt-content allows for the use of
vue components in markdown pages

#### AppLinkContent
Inline component for proper typesetting and handling of links (mainly for termportalen.no)

Properties:
- ``to`` (required): URL that the hyperlink points to
- ``desc``(required): Description of the link

Use example:
::BlockQuote
```
:AppLinkContent{to="termportalen.no" desc="Termportalen hjemmeside"}
```
::

#### BlockQuote
Block component for block quotes

Use example:
::BlockQuote
```
::BlockQuote
Content
::
```
::

#### HeadingTp
Block component for advanced formatting of headings

Properties:
- ``level`` (required): heading level. options: ``h1``, ``h2``, ``h3``, ``h4``, ``h5``, ``h6``
- ``headingId``: Custom ID for heading element
- ``headingClass``: classes for heading element

Use example:
::BlockQuote
```
::HeadingTp{level="h2" headingId="customId" headingClass="text-3xl font-semibold"}
Heading text
::
```
::

#### PlainList
Block component for lists without bullet points or numbers.

Use example:
::BlockQuote
```
::PlainList
- Entry 1
- Entry 2
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

### Backup
Different kinds of data are stored on the backupped (by IT) drive `/data/` on `termwiki_prod`

#### Mediawiki mariadb
- `data/backup/mariadb/wiki`
- Contains mariadb database dumps
- One dump created per day
- Only one dump per month is retained for months that are not recent

#### Mediawiki images
- `data/backup/images`
- Mediawiki images folder is `rsynced` to backupped folder
- Folder contains also contains deleted, derived etc. files

#### Fuseki snapshots
- `data/backup/fuseki`
- contains monthly fuseki prod snapshots to be able to revist historical data
- cronjob for snapshot on the fuseki runs on the 8th of each month
- moving snapshot to backup dir is currently a manual process
