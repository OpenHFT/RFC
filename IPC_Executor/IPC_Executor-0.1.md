# IPC-Safe IPC-Savvy Executor Framework

|         |                                                             |
|:------- | ----------------------------------------------------------- |
| Title   | Wire Format API                                             |
| URL     | https://github.com/OpenHFT/RFC/blob/master/IPC_Executor/    |
| Latest  | https://github.com/OpenHFT/RFC/blob/master/IPC_Executor/IPC_Executor-0.1.md |
| Editor  | Ben Cotton                                                  |
| License | Apache 2.0                                                  |
| Change Process | Users issue Pull Requests for the Editor's consideration. |
| Status  | Raw.                                                        |

# Goals
An RFC which embraces the history/similarity/familiarity of MT-Safe  (Thread-oriented,single JVM) java.util.concurrent Executor framework.
Empowering end-users with a brand new IPC-safe (Process-oriented, grid-oriented, multiple JVMs ) capability.

## Initial Description

OpenHFT RFC:     Chronicle IPC-Safe IPC-Savvy Executor Framework

This RFC calls for the delivery of a Chronicle API -- which embraces the history/similarity/familiarity of MT-Safe  (Thread-oriented,single JVM) java.util.concurrent Executor framework --   empowering OpenHFt end-users with a brand new IPC-safe (Process-oriented, grid-oriented, multiple JVMs )  capability.

E.g.   Just as today's j.u.c API you can configure a single JVM with a BlockingQueue-backed ThreadPoolExecutor that manages the dispatch and harvesting of FutureTasks with MT-safe concurrency, the new OpenHFT API will empower the Chronicle capability to configure a grid of multiple  JVMs with a ChronicleBlockingQueue-backed ChronicleGridPoolExecutor that manages the dispatch and harvesting of ChronicleFutureTaks (ChronicleCallable, ChronicleRunnable) with IPC-safe, IPC-savvy concurrency.

*TODO This need to be made more language and library neutral to decribe the requirement that any compliant library could support.*

## Description
{A description}

## More details description
{A more detailed description}

### Additional details
{Additional Details}

## References

{Add any references or related RFC's here}





