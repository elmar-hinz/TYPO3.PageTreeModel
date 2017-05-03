============================================
TYPO3 Data Model of Page Tree and Root Lines
============================================

Basics
======

Tables in general
-----------------

All tables have the fields **uid** and **pid**. The *uid* is the
primary key. The *pid* connects the record to the page tree by
pointing to a **uid** of a page record. By surprise this principle
holds true for the page records themselves.

Basics of the page tree
-----------------------

The parent child relations of pages are specified within the table
**pages** containing one entry per page. This table also stores
the page title and many other settings related to a page. It doesn't
store the page content. The localisation of the page tree is stored
into the table **pages_language_overlay**.

The **pid** of a page record points to another record, the
**parent record**. The graph modeled by this relation is a tree, the
**page tree**. The overall parent page is addressed as **root page**.

Basics of translation handling
------------------------------

In general translated records are stored into the same table as
the default records. The translations are distinguished by the
field **sys_language_uid**.

There always must be a record in the default language to contribute
**uid** and **pid** for the relations. The **sys_language_uid** of
this records is zero. The translated records refer to the **uid**
of the default record by the pointer **I10n_parent**.

When a translation is loaded from the database, the default record
contributes **uid** and **pid**, while the translated record
contributes the translated text.

Basics of translation handling of pages
---------------------------------------

For historical reasons the translation of **pages** is done a little
differently. The translated records are stored into a second table
**pages_language_overlay**. In this case the **pid** refers to the
**uid** of the default records in **pages**.

Basics of workspace versions
----------------------------

Workspace versions work similar to regular translations. Here the
field **t3ver_id** specifies the version while the field **t3ver_oid**
holds the pointer to the default record.

Summary
-------

There are sets of records for different combinations of language and
workspace versions. In common they refer to a default record. The
default record provides **uid** and **pid** when loaded from the
database. The default record provides the non-workspace version
in the default language.

.. admonition:: Takeaway

    Translations don't influence or change the definition of the page
    tree relations by the fields **uid** and **pid** from **pages**,
    despite *pages_language_overlays* has *uid* and *pid*, too.



