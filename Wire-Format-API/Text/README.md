# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Text Wire Format API                                                    |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/                         |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Text                     |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Text/Text-Wire-Format-API-0.1.md     |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
The Text WFA is a text WFA format based a sub-set of the YAML format.  It is expected that any library which can read YAML can read this text format.

WFA is designed to be high level description for Wire Formats to comply with.  Any Wire Format which complies with this RFC can be accessed through the same API.

This format should be automatically translatable to the Binary format.

# Schema changes
The Text WFA should be able to handle all schema changes in the WFA document.