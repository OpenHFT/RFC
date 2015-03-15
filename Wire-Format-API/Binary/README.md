# Specification

|         |                                                                             |
|:------- | --------------------------------------------------------------------------- |
| Title   | Binary Wire Format API                                                      |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/                 |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Binary           |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Binary/Binary-Wire-Format-API-0.1.md |
| Editor  | Peter Lawrey                                                                |
| License | Apache 2.0                                                                  |
| Change Process | Users issue Pull Requests for the Editor's consideration.            |
| Status  | Raw.                                                                        |

# Goals
The Binary WFA is a binary WFA format based the Text format.

WFA is designed to be high level description for Wire Formats to comply with.  Any Wire Format which complies with this RFC can be accessed through the same API.

This format should be automatically translatable to the Text (human readable) format.

# Schema changes
The Binary WFA should be able to handle all schema changes in the WFA document.