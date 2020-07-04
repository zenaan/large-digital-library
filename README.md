# Design for a Large Personal Digital Library - LPDL

Notes info decisions and schemas for a large library which is shareable and updatable by many people around the world.

Guiding principles: simple, easy, and shareable.

New for 2020, [the specification](spec-0.1.yaml) begins.


## Principles

The principles / functions (purposes) of cataloguing are well summarized at https://en.wikipedia.org/wiki/Cataloguing

  - there are millions of books, too many to store in one directory
    - e.g. a single git archive would be too large
    - 10s of thousands of people might edit different articles (or rather add metadata) simultaneously, on their local repos
    - in other words, integrity/ consistency locking etc could be a big issue
    - so at least a separate git repo (or "sub-repo") per book/movie/etc

  - doomsday library prepping is "fair use" (non legal definition) for archiving proprietary books
    - make sure the concept is:
      - easy to comprehend
      - easy to do
      - easy to share

  - to share the task of creating our "library of everything", or at least the indexing task,
    - requires a shared algorithm to determine the "canonical directory" in a hierarchy,
      within which the content item (book, movie, etc) is to be stored,
    - and a p2p mechanism to share indexing (directory name/ file name) information
    - associated tools for a user:
      - to identify their existing content
      - to move each existing item to "preferred canonical location" or "alternative 'standard' location"
      - to report on content
      - to create a subset of ones content
      - to share ones content with peers sneakernet
      - to share ones indexes and subsets with peers

  - there are so many books, we require a hierarchy

  - the "default/ suggested canonical location" within that hierarchy should be 'useful', even without associated tools
    - e.g. using dewey numbers might match an existing library's storage system, but is probably less useful to the
      average user than a hierarchy of folders which include e.g. author name, and book title

  - likely useful categories:
    - language (e.g. english, french, german, russian, japanese)
    - free/libre licensed, vs proprietary licensed content
    - "standard" categories and sub-categories of fiction, science, academic disciplines etc, see e.g. urls.txt
    - type of content, e.g. books, papers, maps, designs

  - metadata is per item:
    - needs to be updatable per item
    - minimize library-wide edit/create contention
    - localise metadata with its content
    - global indexing can then be readily performed on an individual library
    - different sources of meta data may be available for an item/ added to the library
    - different metadata sources use different schemas, file formats, and possibly
      different encodings
    - UTF-8 encoding is always preferred
    - conversion to UTF-8 should be done prior to storing all text-based metadata in
      the library
    - due to the multitude of possible meta data formats and files,
      a separate folder/directory named METADATA shall be created to store any and all
      metadata for a content item; each metadata file shall be stored in this
      directory

  - metadata is also stored for the library, in the library root metadata/ directory;
    library metadata includes:
    - physical locations of various library "content type" directories or subdirectories
    - logical (filesystem) locations of various library "content type" directories or
      subdirectories (e.g. mount point when mounted)
    - data re copies held (a la git annex for digital copies)
    - some library metadata may be public
      - default is private



## Library names

To facilitate p2p sharing of libraries, i.e.:

  - hierarchy additions (and possibly changes)
  - metadata
  - and the content items stored in a library,

standardization of certain of the names used in the structure and metadata of a library is
critical.

The initial author of this document reads and writes only English.

English shall be the canonical language for library hiearchy and metadata names.

Conversion schemas shall be created and distributed as part of a library,
to facilitate those who do not read English or who prefer another language.

These shall facilitate automatic conversion of a (copy of) an existing library
into a hierarchy which uses non-English names, and possibly even non-english metadata
(field) names (yet to be decided - this might be a bit of work, but let's avoid
implementing anything which limits this possibility).

These conversion schemas shall also facilitate p2p sharing between libraries using
different languages by automating the conversion to the canonical (English) form/ name.

### Library name types

There are 3 primary types of names used within a library as follows:

  1.  library hiearchy names, i.e. directory and file names used to locate a content item
      - these are the directory (or filesystem "folder") names used to organise a
        library
      - to facilitate p2p sharing of libraries and corresponding metadata, these need to
        be standardized and well-known

  2.  content item names (titles) and corresponding descriptions
      - these names are primarily used for searching and for displaying to users
      - these names may (but often do not) correspond to one or more file and or"
        directory names
      - these names are often in one or more languages
        - they require translation in order to be useful to those who read a different
          language

  3.  metadata names
      - these names identify metadata
      - to facilitate p2p library sharing, these must be consistency in metadata names
      - to facilitate sub-dividing overly large metadata files, metadata names must be
        both consistent as well as unambiguously representable as filenames in the widely
        used filesystems:
        - on common GNU/Linux distributions, Ext3 and above
        - on Microsoft Windows 10, NTFS (version XX or above)
        - on SuSE Linux, BTRFS
        - on MacOS, ?
        - on *BSD, ?
      - a schema shall be created to clearly and unambiguously define all metadata names
        (also called fields or field groups etc)

The mnemonic names for different locations and or attributes for a library shall be
standardized, see section "## Decisions".



## Meta data

Metadata facilitates creation of library-wide indexes for searching, category hiearchy creation tools, etc.

  - metadata can be in one or more YAML files
    - if a metadata file is split into sub-files, then their names correspond to the same
      field that would otherwise exist in the corresponding parent file
    - in this way, the full metadata file can be trivially recreated by concatenating the
      sub-files
    - the decision to split a metadata file (or not) is made on a case by case basis
    - to minimize churn in the p2p library sharing network, once a metadata file has been
      split, it should not be recombined except by some wider/ generally accepted
      consensus
  - metadata files for the library are under a the directory named "meta"
    - the top level metadata file for the library is named meta/library.yaml
  - metadata files for a content item are in a sub-directory for that content item named
    "meta"
    - the top level metadata file for a content item is named $CONTENT_ROOT/meta/content.yaml
  - the names of the files correspond to the

`metadata.yaml` contains generic "content item" (i.e. named content, or content item named
by the full hierarchy/ directory path under which this item exists) meta data about the
content item in the current directory, including known public sources for the item:
  - downloadable links/ darknet urls etc for digital content items
  - physical library identifiers for known publically available physical items

`metadata.digital.yaml` contains "content item" meta data about the files/ digital assets
associated with this particular content item, stored in the "library owner"'s digital item collection

`metadata.physical.yaml` contains "content item" meta data about physical copies
associated with this particular content item, stored in the "library owner"'s physical item collection

Examples:
  - original/ alternative filenames
  - alternative / original sources
  - versions
  - formats
  - authors
  - date, date precision (e.g. "circa"/ about, or exact)
  - file hashes - store at least one good hash per file, preferably 2 or 3
  - ISBN
  - encoding
  - publisher
  -

TODO: create a YAML schema/ template for `metadata.yaml`

To some degree, we are creating a relational database - for example, many library items
will be available from multiple physical libraries, but we don't want to repeat all the
information about a particular physical library, in the metadata.yaml file for every
single item available at a particular physical library.
  - so physical library information must be stored elsewhere in the hierarchy
  - and a "library ID" used to identify physical (and digital) libraries
  - only the library ID (which should be human-readable and useful so a human can
    identify the library from its ID) is used in the metadata files for an item
  - this way, when phone numbers and other contact details, and other information about
    a library changes over time, it only changes in one place



## Tools

  - git, git annex, git archive
  - see https://git.wiki.kernel.org/index.php/SubprojectSupport
  - do not use git subtree for large sub projects / submodules - the idea is to create smaller, more manageable repos, easy p2p etc

  - to facilitate p2p sharing of "authoritative library hiearchy structure changes", a lot
    of proposed changes is kept
  - a librarian chooses (specifies) the relationship they choose to have with every peer
  - by default, unless specified, a library only accepts "simple" changes/ additions from
    their peers
  - some peers may be considered "authoritative" in one or more ways
  - for example, addition of two new sub-categories, and removal of an existing category,
    could be combined to improve the hiearchy, but a particular librarian may wish to be
    careful and circumspect about accepting such changes; the software should facilitate
    this, and should by default not accept destructive or "significant" changes
  - in fact, once the idea of a log of changes is implemented, and that all significant
    changes go through this log, the most optimal p2p sharing of changes may be achieved,
    with the maximum of control given to each individual librarian; this is the ideal
    which should be achieved
  - if all structure changes are only done "via the library log", then we shall be well on
    the way to achieving this level of control and auditability of library changes


## Hierarchy components/ categories

Proposed levels/ categories/ hierarchy components:
  - content type
  - language
    - use 2 or 3 letter ISO code using ascii chars?
    - use the word in native language characters?
      - no, since we need a definitive canonical suggested location in the hierarchy
      - additional aliases can be valid of course
      - certain aliases may become well known/ shared - if widely desired, the library p2p system will support this of course
  - license type - floss, proprietary
  - fiction, non-fiction
  - category, e.g. physics
  - sub-category, e.g. astrophysics
  - author, director, anonymous
  - title/content canonical name
  - editions/versions - not sure about this?
    - for e.g. multi-volume books, separate sub-dir for each edition would be good
    - for movies e.g. hollywood-blockbuster and its sequels, separate subdirs makes sense
    - for single-file content, e.g. PDF book, image, different versions might go nicely in a single directory
    - perhaps leave this up to the librarian who creates this content index and associated metadata? perhaps not?

Content types:
  - books
  - magazines/ periodicals
  - music
  - non-music audio
  - movies
  - documentaries
  - images
  - software (distributions have their package management systems, so perhaps library only for proprietary software)



<file name="comparison-notes.md">


## Suggested canonical hierarchy

This is the suggested universal library directory storage hierarchy, pending further
research and discussion.

See also spec-0.1.yaml


    LIBRARY_ROOT/
      metadata/
      peers/
        $PEER_TYPE/
          $PEER_GROUPING_OR_COUNTRY_CODE/
            $id_or_name/
              metadata.yaml
              pub.key
              logo.svg
              logo.png
              ...

      git_repos/
        linux/
        git/
        apache/

      os_repos/
        debian/
        redhat/

      $LANGUAGE/
        $CONTENT_TYPE/
          fiction/
            $CATEGORY/
              $SUB-CATEGORY/
                $AUTHOR/
                  $TITLE/
                    [$FORMAT/]
                      [$FOLDER/]
                        [$SUB_FOLDER/]
                          $ITEM_NAME
          ...
          non-fiction/
            $CATEGORY/
              $SUB-CATEGORY/
                $AUTHOR/
                  $TITLE/
                    [$FORMAT/]
                      [$FOLDER/]
                        [$SUB_FOLDER/]
                          $ITEM_NAME
          ...
          unspecified/
            $CATEGORY/
              $SUB-CATEGORY/
                $AUTHOR/
                  $TITLE/
                    [$FORMAT/]
                      [$FOLDER/]
                        [$SUB_FOLDER/]
                          $ITEM_NAME
          ...


At any level, an e.g. alphabetical split can be used/specified by the user; e.g. Debian's
package pools, which also do a `lib{a..z}/` split.

$LANG or $LANGUAGE is always an [IETF BCP 47 language
tag](https://en.wikipedia.org/wiki/IETF_language_tag).



## Library divisions and mnemonics

In order to store millions of content items (books, documentaries, papers, photos, works
etc) in a way which is useful to humans browsing the library filesystem, various levels of
division are required, from (primary) language, to genre or category (and one or more
levels of sub-genre or sub-category) and possibly other divisions such as license and
"reality" or whether the item is fiction, non-fiction or unspecified as to whether it is
fiction or non-fiction.

When naming an indeterminate level of the hiearchy of a library, a "mnemonic" is often
used, e.g. $LANGUAGE or $AUTHOR.

    English name   Standard mnemonic  # other info
    -------------- ------------------ ----------------------------------------------------------------
    language       $LANGUAGE          # [english | french | etc]
    content_type   $CONTENT_TYPE      # books, videos, music, photos, maps etc (see below)
    category       $CATEGORY
    category       $SUB_CATEGORY
    category       $SSUB_CATEGORY     # do we need 3 or 4 category sub-divisions?
    category       $SSSUB_CATEGORY    # do we need 3 or 4 category sub-divisions?
    author         $AUTHOR            # for movies, this would be the produce, for artworks, the artist, etc
    title          $TITLE             # title for this content
    format         $FORMAT            # OPTIONAL; the type/format of this file (e.g. text, pdf, mp4, folder)
    folder         $FOLDER            # OPTIONAL; a folder in which content items are contained
    sub_folder     $SUB_FOLDER        # OPTIONAL; a sub-folder in which content items are contained
    item_name      $ITEM_NAME         # filesystem name for this content

The library fully qualified name for an item might look like one of the following
(in mnemonic form):

    $LIBRARY_ROOT/$LANGUAGE/$CONTENT_TYPE/$CATEGORY/$SUB_CATEGORY/$AUTHOR/$TITLE/$ITEM_NAME
    $LIBRARY_ROOT/$LANGUAGE/$CONTENT_TYPE/$CATEGORY/$SUB_CATEGORY/$AUTHOR/$TITLE/$FORMAT/$ITEM_NAME
    $LIBRARY_ROOT/$LANGUAGE/$CONTENT_TYPE/$CATEGORY/$SUB_CATEGORY/$AUTHOR/$TITLE/$FORMAT/$FOLDER/$ITEM_NAME
    $LIBRARY_ROOT/$LANGUAGE/$CONTENT_TYPE/$CATEGORY/$SUB_CATEGORY/$AUTHOR/$TITLE/$FOLDER/$SUB_FOLDER/$ITEM_NAME

The exact hiearchy/ folder structure is up to the librarian to decide upon, and is often a
matter of preference and or of optimising the hierarchy so that there are not too many
files at any particular level of the hiearchy.

which for example could look something like one of these:

    $LIBRARY_ROOT/english/books/law/dictionaries/Henry_Campbell_Black/Blacks_1910/Blacks_1910.djvu.txt
    $LIBRARY_ROOT/english/books/law/dictionaries/Henry_Campbell_Black/Blacks_1910/text/Blacks_1910.txt

Other library location mnemonics:

  - $LIBRARY_ROOT - root directory of a library, can be named anything, end-user chosen;
    often "library"

  - $CONTENT_ROOT - directory containing a content item(s)

### Optional possible additional hierarchy divisions

A possible additional hierarchy division could be $LICENSE, although as e.g. books come out
of copyright and go into the public domain, the license for an item changes automatically,
and this can change with jurisdiction, and so license is probably best described within
the metadata as "published: year: ..." etc. Similarly "claimed charge for distribution".
On the other hand, human dignity demands we not glorify the mundane.

Another possible additional hierarchy division could be $REALITY - i.e. one of fiction,
non-fiction, or unspecified, although this is again sometimes a judgement call, so
"fiction" might best be a top level alternative $CATEGORY, with its own sub-category
hierarchy, e.g. fiction/romance fiction/western fiction/sci-fi etc ?



## Technical details / decisions

Decisions made unless good arguments to the contrary arise. Most still need detailed
fleshing out/ YAML schema specification.

  - content type
  - each book, audio cd, movie etc shall be in its own directory, inside a hierarchy
  - the metadata file for a content directory/folder shall be named metadata.yaml
  - file names:
    - only alphanumeric characters?
    - in any language?
    - no spaces in any part of the hierarchy of folder and file names
      - the metadata may include e.g. "original filename: ..."
    - minimal punctuation
    - no control chars
    - be explicit about what's allowed
      - rather than disallow certain chars and have to add more disallowed chars later
      - much easier to add "more allowed chars" than to remove chars already being used!

  - characters allowed in file names:
    - `[a-zA-Z]`
    - `[0..9]`
    - `: , . = + - _ [ ] % $ @`
    - other chars, like em-dash, en-dash etc ? TODO
    - latin, asian and other languages ? TODO

  - separate meta data for digital items, from meta data for physical items
    - metadata.yaml for generic content item metadata
    - metadata.digital.yaml for metadata about digital copies of the items
    - metadata.physical.yaml for metadata about physical copies of the items
    - by default, do not share metadata.physical.yaml nor metadata.digital.yaml
      - optionally share this info on a per-item (i.e. per metadata.physical.yaml file
        basis)
      - e.g.:

        share: no # default if not specified
        share: public # share with anyone who wants
        share: group:my_friends # share with my group called "my_friends"
        share: friend:friend1, friend:friend2 ... # share with friend1 and friend2
        share: friends:"friend1, friend2 ..." # share with friend1 and friend2

### Formats and encodings

The canonical encoding for text data is UTF-8.
  - Refer http://utf8everywhere.org/
  - Should "normal language" unicode chars be allowed in filenames, only ASCII, or some other subset?

Binary files:
  - Binary files are as they are
  - is LE vs BE/big end encoding relevant ?

Text files:
  - Should be UTF-8 encoded where possible.
  - When a text file is not UTF-8 encoded, and the encoding is known, the encoding should be identified in the metadata.yaml file.

File system:
  - Prefer a file system with utf-8 encoding of file names.




<file name="content-types.md">

<file name="todo.md">


<!-- vim: tw=90 ts=2 sw=2 expandtab
