# Specification

|         |                                                                         |
|:------- | ----------------------------------------------------------------------- |
| Title   | Chronicle Engine RFC                                                    |
| Parent  | https://github.com/OpenHFT/RFC/blob/master/Chronicle                    |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Engine             |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Chronicle/Engine/Chronicle-Engine-0.1.md |
| Editor  | Peter Lawrey                                                            |
| License | Apache 2.0                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration.        |
| Status  | Raw.                                                                    |

# Goals
An RFC for library specific requirements for Chronicle Engine. See the parent RFC for cross library requirements.

## Use of Chronicle Service Protocol
All services in Chronicle Engine can be identified by the use of URIs and relative URIs to identify it's services. [Service URI](https://github.com/OpenHFT/RFC/blob/master/Services/URI/)
The access mechanism for this service can be identified via `csp://` though the Service URI makes this optional. e.g. The following URIs are valid addresses
```
csp://hostname:12345/service-group/service-name
csp://hostname/service-group/service-name
csp:///service-group/service-name
/service-group/service-name
service-group/service-name
service-name
```

When a relative service name is provided, the context should provide the prefix of the address, hostname and/or port.
