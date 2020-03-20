# SliTaz wiki

This is a copy of the original [SliTaz wiki].
It also was copied to an external (relative to the SliTaz project) repository in order to protect the documentation from accidental deletion.

All pages (so far only from the English part of the wiki) were not only copied, but also rewritten from the [Dokuwiki] format to the [reStructuredText]  (ReST) format.
This format supports [Sphinx] and allows you to generate documentation from it in different formats.
Among these formats, we are primarily interested in the HTML and PDF formats.

Yes, you can download the entire wiki for yourself either as a set of HTML pages, or as the single 500-page PDF file.


## Contributing

1. You can contribute to the original wiki.
   Correct errors and inaccuracies, update the documentation.
   All your changes will be transferred as far as possible to this wiki.
2. You can also contribute to this wiki.
   All pages were rewritten from the Dokuwiki format to the ReST format and additional semantic markup was added, which allows [Sphinx].
   Therefore, only changes in the format and additional markup are accepted here.

Keyboard shortcuts are highlighted with `:kbd:`, a program — with `:program:`, a command — with `:command:` and so on.
ReST is not attached only to HTML like [Markdown], instead it allows you to write a single documentation from which HTML, PDF, ePub, etc. will be obtained.
Please do your pool requests here for better markup in HTML and PDF.

In addition, tiny changes were made here to normalize the terms, for example, all “Slitaz” were replaced by “[SliTaz]”, “cdrom” (not path, not device `/dev/cdrom`) — by “[CD-ROM]”, “iso” (in the meaning “disk image”; not in file name) — by “[ISO]”, etc.
Such tiny changes are also accepted here.

By the way, also for this wiki a vector illustration was created for the article "[Boot tree]" using [draw.io].


## Licensing

Unfortunately, I do not know under which licenses the original articles were written. Please let me know if you have any information about this.

All articles from this wiki are freely distributed under original licenses. Let me know if you are the author of the original article and do not want your article to be freely available, and it will be deleted from here (I will be very sorry to do this).

If the license information is still not clear, then the license that [Dokuwiki] uses by default will be valid:

> Except where otherwise noted, content on this wiki is licensed under the following license: [CC Attribution-Share Alike 4.0 International][by-sa-4.0]

Soon, information about authors and contributors will be posted here on every page.


## Notes

This repository contains not only pages in ReST format, but also pages in the original [Dokuwiki] markup are also available so that you can easily find the differences between the page that went into this wiki and the page that might possibly be updated in the original wiki.

Also, Manuals were added here, because the links are not given to the pages of Manuals, but only the source of the pages.
It's nice to have them here too.

Many (but not all) links were checked for availability and, if necessary, were replaced with a copy from the web archive.

Please make links more accessible — replace wherever you see "here" with the page title.
Without special need, do not reassign the title of the reference link (`:ref:`) — in a paper PDF book it will be difficult to understand where the link leads if the name of the chapter to which it refers is different.

Although at first glance it may seem that the markup is already a lot, but it is not.
The markup helps you immediately understand what is being said.
The names of programs, commands, paths are marked…
The idea.
Although the names of programs and commands may be simple English words, they will not be translated into other languages.
All untranslatable terms should be marked too, such as *wok* and *cook*, for example.


## Read the Docs

[![Documentation Status](https://readthedocs.org/projects/slitaz-wiki/badge/?version=latest)](https://slitaz-wiki.readthedocs.io/?badge=latest)

  * [Read online]
  * [Download HTML.zip]
  * [Download PDF]


[SliTaz wiki]:      http://doc.slitaz.org/
[Dokuwiki]:         https://www.dokuwiki.org/dokuwiki
[reStructuredText]: https://docutils.sourceforge.io/rst.html
[Sphinx]:           https://www.sphinx-doc.org/en/master/
[Markdown]:         https://en.wikipedia.org/wiki/Markdown
[SliTaz]:           https://en.wikipedia.org/wiki/SliTaz
[CD-ROM]:           https://en.wikipedia.org/wiki/CD-ROM
[ISO]:              https://en.wikipedia.org/wiki/ISO_(disambiguation)
[Boot tree]:        http://doc.slitaz.org/en:guides:bootguide
[draw.io]:          https://app.diagrams.net/
[by-sa-4.0]:        https://creativecommons.org/licenses/by-sa/4.0/deed.en

[Read online]:       https://slitaz-wiki.readthedocs.io/
[Download HTML.zip]: https://slitaz-wiki.readthedocs.io/_/downloads/en/latest/htmlzip/
[Download PDF]:      https://slitaz-wiki.readthedocs.io/_/downloads/en/latest/pdf/
