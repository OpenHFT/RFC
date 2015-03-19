# Specification

|         |                                                                     |
|:------- | ------------------------------------------------------------------- |
| Title   | Service Messages RFC                                                |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Service/Messages/Service-Messages-0.1.md |
| Editor  | Peter Lawrey                                                        |
| License | Apache 2.0                                                          |
| Change Process | Users issue Pull Requests for the Editor's consideration.    |
| Status  | Raw.                                                                |

# Goals
RFC on how messages can be sent for routing to actors.

# Unique resource identifier
All resource map to a URI.  This URI can be used to access any service if known. See [Service URI](https://github.com/OpenHFT/RFC/blob/master/Service/URI/) for more details.

# Examples
The following section includes example of how a destination and it's messages can be sent.

## Message representation
Each message is prefixed by a length.  The length can include whether the content is meta data or data.
Large blocks of data can be fragmented into multiple sections

## ABNF representation
See [Size Prefixed Blob RFC](https://github.com/OpenHFT/RFC/blob/master/Size-Prefixed-Blob/)
```abnf
stream              = *messages
messages            = meta-data *(*not-ready-data data)
```

## Message dump representation
A typical message contains `meta-data` followed by a stream of `data` messages.

### Example
This is an example of a service lookup message

```YAML
--- !!meta-data
csp:///service-lookup
tid: 149873598325 # unique transaction id to a reply to on the channel sending the request.
...
--- !!data
lookup: { relativeUri: Colours }
```

The response could look like.

```YAML
--- !!meta-data
tid: 149873598325
...
--- !!data
lookup-reply: { relativeUri: Colours, uri: "csp://my-hostname:port//Colours" }
```

## Implementation
The intent of this structure is to easily map to asynchronous method calls.
The meta data determines which actor to pass the request to.
The data contains the messages to pass to the Actor. In Java this could look

```java
public interface ServiceLookup {
    public Future<LookupReply> lookup(Lookup lookupRequest);
}

public interface Lookup {
    public String getRelativeUri();
    public void setRelativeUri(String relativeUri);
}

public interface LookupReply {
    public String getRelativeUri();
    public void setRelativeUri(String relativeUri);
    public URI getUri();
    public void setUri(URI uri);
}
```

## Future use
In the future, use of the `user-name@`, `?query` and `#fragment` notation may be used.

# References
[URI Wikipedia](http://en.wikipedia.org/wiki/Uniform_resource_identifier)
