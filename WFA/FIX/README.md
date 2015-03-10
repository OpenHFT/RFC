# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | JSON Wire Format API                                                    |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/WFA/                         |
| URL     | https://github.com/OpenHFT/RFC/blob/master/WFA/FIX                      |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/WFA/FIX/WFA-FIX-0.1.md       |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
The FIX WFA is a WFA format based the FIX Protocol.

WFA is designed to be high level description for Wire Formats to comply with.  Any Wire Format which complies with this RFC can be accessed through the same API.

This format should be automatically translatable to the Text (human readable) format.

# Schema changes
The FIX WFA supports a subset of schema changes allowed by the standard FIX Protocol.  This includes

 - fields out of order
 - optional fields
 - extended fields ids not in the standard.
 - field ids can appear multiple times.