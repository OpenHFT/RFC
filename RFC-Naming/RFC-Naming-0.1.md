# RFC Naming

|      |                                                          |
|:---- | -------------------------------------------------------- |
| URL | https://github.com/OpenHFT/RFC/blob/master/RFC-Naming/RFC-Naming-0.1.md |
| Editor | Peter Lawrey |
| License | Apache 2.0 |
| Change Process | Users issue Pull Requests for the Editor's consideration. |
| Status | Raw. |

# Goals
A guide to describing how the naming of RFCs, names of terms, and common words used in APIs

# A term should be specific or deliberately ambiguous
In this section "term" may refer to a name of something, the name of an API, or it's acronym, a class name, a method name.

Care should be taken to use either
 - specific term or acronym which isn't, similar but not the same as, an established term or acronym.
 - use a generic term when the term in a generic sense meant. i.e. don't attach a specific meaning to a generic term.

In naming, where possible
  - use a standard term as closely as possible to how it is already used in other standard products or RFCs.
  - if the meaning or functionality expected is different, use a new term so as it is not confused with an established term.
  - if a term has a proprietary usage, associated with only one vendor, it many make sense to use a new generic term instead. A reference to the vendor specific term can be added to the RFC.

Where there is a possible source of confusion, try to highlight this in each RFC.

# Abbreviated Names of OpenHFT's RFCs
OpenHFT's RFCs are made unique by their short identifiers.  These identifiers should be in TitleCase except for acronyms which are in UPPER-CASE.
The use of a minus/ASCII hyphen or "-" is preferred as a separator in the short name of an RFC, rather than space or any other symbol.

# Action names in methods
Where possible, the use of the following names should mean

 - "get" To get a value, possibly associated with a key or index.  This should not create a new object or alter the underlying data structure, unless mimicking a container which virtually has more data than is actually stored.
 - "set" this should set a field, or a value associated with a key or index. A "set" method might validate the argument provided.
 - "create" To always create a new object.
 - "acquire" to get or create as required.
 - "on" handle an event.

## Other terms used which have similar meaning in other documented use.

## Terms which used elsewhere for a different meaning.

## References

[IETF RFCs](http://www.rfc-editor.org/rfc-index.html)

[ABNF Wikipedia](http://en.wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_Form)
{Add any references or related RFC's here}