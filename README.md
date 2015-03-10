# RFC
RFC's proposed under the OpenHFT banner. They don't have to relate to HFT but they must be open.

A guiding principle should be simplicity.  This make it easier to understand and implement, but is also more likely to be fast.

# Raw RFCs

| RFC Code                                                                  | Description               |
|:------------------------------------------------------------------------- | ------------------------- |
| [Caching](https://github.com/OpenHFT/RFC/blob/master/Caching)             | Data Caching API          |
| [IPC-Executor](https://github.com/OpenHFT/RFC/blob/master/IPC-Executor)   | IPC Base Executor         |
| [SPB](https://github.com/OpenHFT/RFC/blob/master/SPB)                     | Size Prefix Blog          |
| [WFA][https://github.com/OpenHFT/RFC/blob/master/WFA)                     | Wire Format API           |

# Meta RFCs

| RFC Code                                                                  | Description               |
|:------------------------------------------------------------------------- | ------------------------- |
| [RFC-Naming](https://github.com/OpenHFT/RFC/blob/master/RFC-Naming)       | RFC Naming                |
| [RFC-Template](https://github.com/OpenHFT/RFC/blob/master/RFC-Template)   | RFC Template              |

## Versioning

Each Specification is held in a directory which has a file for each version. Each specification will have a short code with the first version being 0.1. (followed by 0.2, 0.3 ...)
It is prompted to version 1.0 when released followed by 1.1, 1.2, 1.3 ... for minor clarifications and 2.0, 3.0, 4.0 ... for major enhancements.
Each standard must be backward compatible with previous versions.  For a breaking change, start a new specification.

A version can be created or added by forking the repo and issuing a pull request for the changes made.

## Naming of RFCs and terms
Refer to the latest version in the RFC-Naming directory.

## Template
Refer to the latest version in the RFC-Template directory.

# License

Any contributions you make are on the understanding that this will be under an Apache 2.0 license.  See the LICENSE file for details.

# Related References

IETF RFC http://www.rfc-editor.org/rfc-index.html

Digistan Spec Template http://spec.digistan.org/

ZeroMQ RFCs http://rfc.zeromq.org/main:about

