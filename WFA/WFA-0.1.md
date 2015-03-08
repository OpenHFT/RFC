# Wire Format API

|:----|-----------------------------------------------------------|
| URL | https://github.com/OpenHFT/RFC/blob/master/WFA/WFA-0.1.md |
| Editor | Peter Lawrey |
| License | Apache 2.0 |
| Change Process | Users issue Pull Requests for the Editor's consideration. |
| Status | Raw. |

## Goals
WFA is designed to be high levell description for Wire Formats to comply with.  Any Wire Format which complies with this RFC can be accessed through the same API.

The intent of the API is to produce a wire format which is;
 - human readable (or can be converted to a human readble format), 
 - cross platform
 - resilient to schema changes in the data encoded.

## Schema changes
Where possible, the format should be tolerant of 
 - adding fields.
 - removing fields.
 - changing the order of fields.
 - changing the type of a scalar field.
 - using a different composite data type on the sender and reciever.
 
## Field based data
Each piece of data is associated with a name, number or implicitly ordered field. 

## Data based encoding
The primary goal of the wire format is to transfer data rather than attempt to be a faithful serialization format.

# ABNF description

```
stream              = optional-header *field-with-value optional-footer
field-with-value    = field data
data                = scalar | composite-data
scalar              = text | number | date-time
composite-data      = *field-with-value
```

## Planned RFCs
The following Wire Formats are plannd
 - Text Wire Format - a simplified Yaml format
 - JSon Wire Format - a format derived from JSon.
 - Binary Wire Format - a binary format of the Text Wire Format
 - RAW Wire Format - a terse binary value only format.
 - FIX Wire Format - a wire format based on FIX.

## References

[RFC:SPB - Size Prefixed Blob](https://github.com/OpenHFT/RFC/blob/master/SPB)

[ABNF Wikipedia](http://en.wikipedia.org/wiki/Augmented_Backus%E2%80%93Naur_Form)
