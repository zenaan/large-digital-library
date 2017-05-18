# Design for a Large Digital Library
Notes info decisions and schemas for a large library which is shareable and updatable by many people around the world.

Guiding principles: simple, easy, and shareable.


## Principles

   -  there are millions of books, too many to store in one directory
      -  e.g. a single git archive would be too large
      -  10s of thousands of people might edit different articles (or rather add metadata) simultaneously, on their local repos
      -  in other words, integrity/ consistency locking etc could be a big issue
      -  so at least a separate git repo (or "sub-repo") per book/movie/etc

   -  doomsday library prepping is "fair use" (non legal definition) for archiving proprietary books
      -  make sure the concept is:
         -  easy to comprehend
         -  easy to do
         -  easy to share

   -  to share the task of creating our "library of everything", or at least the indexing task,
      -  requires a shared algorithm to determine the "canonical directory" in a hierarchy,
         within which the content item (book, movie, etc) is to be stored,
      -  and a p2p mechanism to share indexing (directory name/ file name) information
      -  associated tools for a user:
         -  to identify their existing content
         -  to move each existing item to "preferred canonical location" or "alternative 'standard' location"
         -  to report on content
         -  to create a subset of ones content
         -  to share ones content with peers sneakernet
         -  to share ones indexes and subsets with peers

   -  there are so many books, we require a hierarchy

   -  the "default/ suggested canonical location" within that hierarchy should be 'useful', even without associated tools
      -  e.g. using dewey numbers might match an existing library's storage system, but is probably less useful to the
         average user than a hierarchy of folders which include e.g. author name, and book title

   -  likely useful categories:
      -  language (e.g. english, french, german, russian, japanese)
      -  free/libre licensed, vs proprietary licensed content
      -  "standard" categories and sub-categories of fiction, science, academic disciplines etc, see e.g. urls.txt
      -  type of content, e.g. books, music, audio, movies, documentaries, software

   -  Meta data is per item:
      -  needs to be updatable per item
      -  minimize library-wide edit/create contention
      -  localise metadata with its content
      -  global indexing can then be readily performed on an individual library



## Meta data

Metadata facilitates creation of library-wide indexes for searching, category hiearchy creation tools, etc.

metadata.yaml contains content meta data about the files in the current directory

Examples:
   -  original/ alternative filenames
   -  alternative / original sources
   -  versions
   -  formats
   -  authors
   -  date, date precision (e.g. "circa"/ about, or exact)
   -  file hashes - store at least one good hash per file, preferably 2 or 3
   -  ISBN
   -  encoding
   -  publisher
   -

TODO: create a YAML schema/ template for metadata.yaml



## Formats and encodings

The canonical encoding for text data is UTF-8.
   -  Refer http://utf8everywhere.org/
   -  Should "normal language" unicode chars be allowed in filenames, only ASCII, or some other subset?

Binary files:
   -  Binary files are as they are
   -  is LE vs BE/big end encoding relevant ?

Text files:
   -  Should be UTF-8 encoded where possible.
   -  When a text file is not UTF-8 encoded, and the encoding is known, the encoding should be identified in the metadata.yaml file.

File system:
   -  Prefer a file system with utf-8 encoding of file names.



## Tools

   -  git, git annex, git archive
   -  see https://git.wiki.kernel.org/index.php/SubprojectSupport
   -  do not use git subtree for large sub projects / submodules - the idea is to create smaller, more manageable repos, easy p2p etc



## Hierarchy components/ categories

Proposed levels/ categories/ hierarchy components:
   -  content type
   -  language
      -  use 2 or 3 letter ISO code using ascii chars?
      -  use the word in native language characters?
         -  no, since we need a definitive canonical suggested location in the hierarchy
         -  additional aliases can be valid of course
         -  certain aliases may become well known/ shared - if widely desired, the library p2p system will support this of course
   -  license type - floss, proprietary
   -  fiction, non-fiction
   -  category, e.g. physics
   -  sub-category, e.g. astrophysics
   -  author, director, anonymous
   -  title/content canonical name
   -  editions/versions - not sure about this?
      -  for e.g. multi-volume books, separate sub-dir for each edition would be good
      -  for movies e.g. hollywood-blockbuster and its sequels, separate subdirs makes sense
      -  for single-file content, e.g. PDF book, image, different versions might go nicely in a single directory
      -  perhaps leave this up to the librarian who creates this content index and associated metadata? perhaps not?

Content types:
   -  books
   -  magazines/ periodicals
   -  music
   -  non-music audio
   -  movies
   -  documentaries
   -  images
   -  software (distributions have their package management systems, so perhaps library only for proprietary software)


## Comparison notes

There are other classification systems in use by physical libraries such as
*LCC / Library of Congress Classification* https://en.wikipedia.org/wiki/Library_of_Congress_Classification
and *Dewey Decimal Classification*.
See a comparison here: https://en.wikipedia.org/wiki/Comparison_of_Dewey_and_Library_of_Congress_subject_classification

These classification systems appear to have divisions which are designed to a fair degree
for optimal physical grouping of objects (e.g. books, maps and CDs) in a physical building
- and so the groupings perhaps don't quite make sense or at least "may not be optimal"
from a pure classification point of view, even though such system are optimal, or at
least convenient, for the storage of thousands of physical items (and in particular
considering physical storage space and space expansion required over time).

Although this Large Digital Library project may facilitate an individual (or small/
community library) to record information about which books they physically hold in their
collection, the primary intention is to optimize for a digital library - that is, for the
storage and indexing of content items as files.

From this "digital storage" point of view, the optimal filesystem/ directory layout is one
which is most useful to a lay person browsing their file system.

There is no need to group different [sub-]categories merely for storage purposes - we can
separate every [sub-]category that ought be separated since this will not cause a physical
storage sytsem problem. So a full and comprehensive folder/ directory/ file system
hierarchy is therefore preferred over a hierarchy naming system optimised for storage of
physical items.



## Suggested canonical hierarchy

This is the suggested universal library directory storage hierarchy, pending further research and discussion:

    $CONTENT_TYPE/
       $LANG/
          floss/
             fiction/
                $CATEGORY/
                   $SUB-CATEGORY/
                      $AUTHOR/
                         $TITLE/
                ...
             non-fiction/
                $CATEGORY/
                   $SUB-CATEGORY/
                      $AUTHOR/
                         $TITLE/
                ...

          proprietary/
             fiction/
                $CATEGORY/
                   $SUB-CATEGORY/
                      $AUTHOR/
                         $TITLE/
                ...
             non-fiction/
                $CATEGORY/
                   $SUB-CATEGORY/
                      $AUTHOR/
                         $TITLE/
                ...



## Decisions

Decisions made unless good arguments to the contrary arise:
   -  content type
   -  each book, audio cd, movie etc shall be in its own directory, inside a hierarchy
   -  the metadata file for a content directory/folder shall be named metadata.yaml
   -  file names:
      -  only alphanumeric characters?
      -  in any language?
      -  no spaces in any part of the hierarchy
         -  the metadata may include e.g. "original filename: ..."
      -  minimal punctuation
      -  no control chars
      -  be explicit about what's allowed
         -  rather than disallow certain chars and have to add more disallowed chars later
         -  much easier to add "more allowed chars" than to remove chars already being used!
   -  characters allowed in file names:
      -  [a-zA-Z]
      -  [0..9]
      -  : , . = + - _ [ ] % $ @
      -  other chars, like latin, asian, em-dash, en-dash etc ? TODO

   -  separate meta data for digital items, from meta data for physical items
      -  metadata.yaml for generic content item metadata
      -  metadata.digital.yaml for metadata about digital copies of the items
      -  metadata.physical.yaml for metadata about physical copies of the items
      -  by default, do not share metadata.physical.yaml nor metadata.digital.yaml
         -  optionally share this info on a per-item (i.e. per metadata.physical.yaml file
            basis)
         -  e.g.:
            share: no # default if not specified
            share: public # share with anyone who wants
            share: group:my_friends # share with my group called "my_friends"
            share: friend:friend1, friend:friend2 ... # share with friend1 and friend2
            share: friends:"friend1, friend2 ..." # share with friend1 and friend2

TODO:
   -  file hashes: the following hashes should be stored in metadata.yaml for each content file present:
      -  SHA3-256
         -  see https://en.wikipedia.org/wiki/SHA-3
      -  CubeHash(i=16,r=16,b=32,f=32,h=512)
         -  see http://cubehash.cr.yp.to/
   -  need to decide on how to identify friends, nodes etc - e.g.:
      -  global block chain id ?
      -  email address (with PGP key for auth) ?
      -  some alternative naming system ?
         https://www.afnic.fr/en/resources/publications/issue-papers/alternative-naming-systems-to-the-dns-2.html
      -  gnunet ?
      -  namecoin ?
      -  some sort of tor/i2p/onion/darknet ID ?


# vim: tw=90
