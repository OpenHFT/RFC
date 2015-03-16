# Specification

|         |                                                                     |
|:------- | ------------------------------------------------------------------- |
| Title   | Service URI RFC                                                     |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Service/URI/Service-URI-0.1.md |
| Editor  | Peter Lawrey                                                        |
| License | Apache 2.0                                                          |
| Change Process | Users issue Pull Requests for the Editor's consideration.    |
| Status  | Raw.                                                                |

# Goals
RFC on how URI should be used in services.

As well as using URI's to uniquely identify a service, a context for URI can be used to specify relative URIs.

# Unique resource identifier
All resource map to a URI.  This URI can be used to access any service if known.

# Relative URI.
When referring service, a relative URI can be used.  This will be relative to a service path to search for a service.

If a service URI starts with a word and does not contain a colon it can form a relative address of a context path. e.g. If the context path is
```
csp://host1:9876/dir/
tcp://host2:58796/services/
```
and the resource is specified as `group/service-name`, the resulting search path will be
```
csp://host1:9876/dir/group/service-name
tcp://host2:58796/services/group/service-name
```

## Future use
In the future, use of the `user-name@`, `?query` and `#fragment` notation may be used.

# References

[URI Wikipedia](http://en.wikipedia.org/wiki/Uniform_resource_identifier)
