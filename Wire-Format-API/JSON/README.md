# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | JSON Wire Format API                                                    |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/                         |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/JSON                     |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/JSON/JSON-Wire-Format-API-0.1.md     |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
The JSON WFA is a WFA format based the JSON format.

WFA is designed to be high level description for Wire Formats to comply with.  Any Wire Format which complies with this RFC can be accessed through the same API.

This format should be automatically translatable to the Text (human readable) format.

# Schema changes
The JSON WFA should be able to handle all schema changes in the WFA document.