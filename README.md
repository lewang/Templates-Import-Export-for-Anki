Templates Import / Export : an Anki Add-on
====
A tool for Anki user to **import / export** **CSS** and **card templates** of all note types.

## Version History

- **Original**: https://ankiweb.net/shared/info/712027367 by Asu4ni
- **Anki 23.12+ fixes**: https://ankiweb.net/shared/info/2032572419 by Shigeyuki (API
  compatibility updates, better error handling, and last directory memory)
- **This version** (Nov 2025): Common epilog support

Functions & Things to notice
----

+ **Export**: choose the directory as the destination for export
  - Each note type has its own folder, containing the CSS and templates of all cards.
  - Each template file is named after the card, containing both the front & back templates.
  - The front & back templates are separated by the **configur**able delimiter.
+ **Import**: choose the directory as the source for import
  - Each folder in the chosen directory will be regarded as each individual note type to be
    imported, matched by the name of the folder.
  - Folders or template files without corresponding note types or cards in Anki will be ignored. The
    add-on won't create note types and cards as a result. Therefore, importing the directory which
    was exported earlier is recommended.
  - An extra CSS file can be present in the source directory (compared to what **Export** outputs),
    regarded as the global CSS. When the CSS file is missing under a note type folder, the global
    CSS will be used if present. The behavior when both are present is **configur**able.
  - An extra common epilog HTML file can be present in the source directory. When present, its
    contents will be appended to both the front and back of every card template, prefixed with a
    marker comment `<!-- common_epilog.html -->`. This is useful for shared JavaScript or other
    common footer content. **[NEW in this version]**
+ **Configure**: some configurable settings in JSON format
  - *delimiter between front and back template*: the demimiter for separating front & back templates
    in each card. Default: `` ```<br> `` (``<br>`` is line break)
  - *CSS file name*: the filename for all CSS files. Default: ``style.css``
  - *common epilog file name*: the filename for the common epilog HTML file. Default:
    ``common_epilog.html``
  - *insert global CSS before individual ones of all note types*: if ``true``, global CSS file will
    be inserted before each note type's CSS file. Otherwise, global CSS will be ignored if there is
    a CSS file for the note type. Default: ``false``
  - *filename extensions for card template files*: an empty string ``""`` for no extension. An
    alternative might be ``".html"``. Default: ``""``
  - *last_dir*: stores the last directory used for import/export operations. The file dialog will
    automatically open to this location on subsequent operations. **[NEW in this version]**

## New Features in This Version

- **Common Epilog Support**: Add a `common_epilog.html` file to append shared content (like
  JavaScript) to all card templates automatically during import
- **Last Directory Memory**: The file dialog remembers your last used directory for faster
  import/export operations
- **Better Error Handling**: Improved error messages when note type or card type names contain
  invalid filesystem characters
- **Anki 23.12+ Compatibility**: Updated API calls (`byName()` → `by_name()`) and Qt imports for
  modern Anki versions

Others
----
+ **This version source on GitHub**: https://github.com/lewang/Templates-Import-Export-for-Anki
+ Original source on GitHub: https://github.com/Asu4ni/Templates-Import-Export-for-Anki
+ Original add-on on AnkiWeb: https://ankiweb.net/shared/info/712027367
+ Anki 23.12+ fixes version: https://ankiweb.net/shared/info/2032572419
