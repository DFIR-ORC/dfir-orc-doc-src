Requirements
============

DFIR ORC requires *administrative* privileges. Indeed, data accessed is privileged.
However, the embedded tool suite does not go through the permission system of Microsoft Windows to access
files. It rather interacts at a lower level, using devices, thus preventing access denials on files already in use, locked, etc.
This enables to collect critical artefacts, such as hives, event logs and pagefiles.

DFIR ORC is tested on the following platforms:

* Microsoft Windows XP SP2 (32 bits)
* Microsoft Windows XP SP3 (32 bits)
* Microsoft Windows Server 2003 (R2) SP3 (32&64 bits)
* Microsoft Windows Vista SP2 (32&64 bits)
* Microsoft Windows Server 2008 SP2 (32&64 bits)
* Microsoft Windows 7 SP1 (32&64 bits)
* Microsoft Windows Server 2008 R2 (64 bits)
* Microsoft Windows 8 (32&64 bits)
* Microsoft Windows Server 2012 (64 bits)
* Microsoft Windows 8.1 (32&64 bits)
* Microsoft Windows 8.1 Update (32&64 bits)
* Microsoft Windows Server 2012 R2 (64 bits)
* Microsoft Windows 10 (32&64 bits)
* Microsoft Windows Server 2016 (64 bits)
* Microsoft Windows Server 2019 (64 bits)

DFIR ORC creates processes for data collection and assigns them to run within a Windows Job Object.
This job object allows to control and monitor resources used by the tools (memory, IO, UI, ...).
DFIR ORC needs to create this job object *and* be able to assign processes to it.

Depending on the deployment method used and Windows versions, it is possible that the ``DFIR-Orc.exe`` process is created within a job
object. When this is the case, the process must be allowed to "break away from the job" (JOB_OBJECT_LIMIT_BREAKAWAY_OK).
If this fails, ``DFIR-Orc.exe`` will attempt to create a nested job object and run processes into this object.

.. admonition:: Warning for Windows 7, XP SP3, Vista, and Windows Server 2003, 2008, 2008 R2
   :class: warning

   In these versions of Windows, a process can be associated with only one job: jobs cannot be nested.
   The ability to nest jobs was added in Windows 8 and Windows Server 2012.
   If ``DFIR-Orc.exe`` is running inside a job object that does not have the permission "break way from job", and nested jobs are not available, then DFIR ORC will attempt to temporarily modify the job to allow its children to break away. If that attempt fails, DFIR ORC will be unable to launch processes and will fail. At this time, there is no known configuration where DFIR ORC fails for this reason but this corner case could still occur. In such a case, an appropriate error message is displayed.

More information on `job objects on MSDN <https://docs.microsoft.com/en-us/windows/win32/procthread/job-objects>`_.


