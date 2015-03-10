# OpenHFT as a JCACHE

|         |                                                             |
|:------- | ----------------------------------------------------------- |
| Title   | Wire Format API                                             |
| URL     | https://github.com/OpenHFT/RFC/tree/master/OHFTaaJCACHE/    |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/OHFTaaJCACHE/OHFTaaJCACHE-0.1.md |
| Editor  | Ben Cotton                                                  |
| License | Apache 2.0                                                  |
| Change Process | Users issue Pull Requests for the Editor's consideration. |
| Status  | Raw.                                                        |

# Goals
OpenHFT as a JCACHE

## Initial Description
*Comment This needs to be made more language and library neutral to describe the requirement that any compliant library could support.*
OpenHFT as a JCACHE.  Potentially = OpenHFT as a Native Platform Language Interoperable IPC Cache (musing only)



E.g.   Just as today's j.u.c API you can configure a single JVM with a BlockingQueue-backed ThreadPoolExecutor that manages the dispatch and harvesting of FutureTasks with MT-safe concurrency, the new OpenHFT API will empower the Chronicle capability to configure a grid of multiple  JVMs with a ChronicleBlockingQueue-backed ChronicleGridPoolExecutor that manages the dispatch and harvesting of ChronicleFutureTaks (ChronicleCallable, ChronicleRunnable) with IPC-safe, IPC-savvy concurrency.


## Description
{A description}

## More details description
{A more detailed description}

### Additional details
{Additional Details}

## References

{Add any references or related RFC's here}





