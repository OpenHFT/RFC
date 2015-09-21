# Specification

|         |                                                                                         |
|:------- | --------------------------------------------------------------------------------------- |
| Title   | Data Bootstrap                                                                          |
| URL     | https://github.com/OpenHFT/RFC/blob/master/Self-Bootstrapping-Data                      |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/Self-Bootstrapping-Data/Self-Bootstrapping-Data-0.1.md  |
| Editor  | Peter Lawrey                                                                            |
| License | Apache 2.0                                                                              |
| Change Process | Users issue Pull Requests for the Editor's consideration                         |
| Status  | Raw.                                                                                    |

# Goals
Data Bootstrap describes how a file or TCP data stream can bootstrap itself.

The header of a master file or start of a strap has a message containing a serializable object.  This object is deserialized
and operation of the stream or files or directory is delegated to this deserialized object.


