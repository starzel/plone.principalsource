Introduction
============

This package provides a queriable sources (vocabularies) that return PAS
users, groups or principals (both users and groups).

They are registered as named vocabularies, so you can do::

  class IMyInterface(Interface):
      users = schema.Choice(title=u"Users",
                            vocabulary="plone.principalsource.Users")
      
      groups = schema.Choice(title=u"Groups",
                             vocabulary="plone.principalsource.Groups")
                            
      principals = schema.Choice(title=u"Principals",
                                 vocabulary="plone.principalsource.Principals")

The underlying source (see source.py) implements the IQuerySource interface
from z3c.formwidget.query. This means that it can be used for a query-select
widget, including the one in plone.formwidget.autocomplete. 

Note
----

Moved from http://svn.plone.org/svn/plone/plone.principalsource/

A note about unicode
--------------------

The source attempts to make it safe to do a __contains__ check, a getTerm()
lookup, and searches using unicode strings. This is somewhat constrained by
the underlying plugins. In particular, the standard ZODBGroups plugin is
incapable of searching for groups with unicode titles or ids, and returns
a list of *all* groups if passed a unicode string. As such, the source
forces all unicode strings used to search for groups to UTF-8 (searching for
users is unaffected).

Also, remember that tokens should be 7-bit ASCII strings. getTermByToken() is
forgiving in that it silently encodes a unicode string to utf-8, but really
you should only pass unicode to this method.

