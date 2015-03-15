# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Raw Wire Format API                                                     |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/                         |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Raw                   |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Wire-Format-API/Raw/Raw-Wire-Format-API-0.1.md |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
The Raw WFA is a format which contains a minimum of meta data.  It is designed to be read with a minimum of overhead.  
This may mean it is smaller than other formats, but without support for compression, it could be larger.

WFA is designed to be high level description for Wire Formats to comply with.  Any Wire Format which complies with this RFC can be accessed through the same API.

Other formats should be abel to be translated to Raw WFA, however conversion back again would require knowledge of the expected data types involved.

# Schema changes
The only schema detail supports is the length of data.  The schema can be extended by appending data, and readers should be able to handle either more or less data than expected.