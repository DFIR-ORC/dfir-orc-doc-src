===================================
Configuring Console Output, Logging
===================================

Some log output is available with different verbosity levels. It can be shown in the console or into a file. Both levels are independent.

Usage
=====

In XML configuration file, the console output and the file output are configured within the ``log`` element.


``log`` Element
===============

*optional=yes, default=N/A*, `parent element: wolf <#wolf-element>`_

The log element can be used to create an optional log file of DFIR ORC execution. This file will be uploaded if an <upload/> element is specified in a :doc:`DFIR ORC local configuration file <orc_local_config>`.

The log message are passing through "sinks" like 'console' or 'file'. To configure log output a sink must be specified.

``Console`` sink element, ``/log:console,...`` Option
------------------------------------------------------

*optional=yes, default=N/A*, `parent element: log`

``level`` Attribute, ``/log:console,level=<Level>,...`` Option
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Log level is one of 'trace', 'debug', 'info' 'error', 'warning', 'critical'.

Default: critical.

``backtrace`` Attribute, ``/log:console,backtrace=<Level>,...`` Option
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Specify a log level which will trigger a log backtrace which will contain logs up to level 'debug'.

Value is one of 'trace', 'debug', 'info' 'error', 'warning', 'critical', off.

Default: 'off'.

``File`` sink element, ``/log:file,...`` Option
------------------------------------------------

*optional=yes, default=N/A*, `parent element: log`

The logging can be written to the file at the end of the tool execution.
This implies that tool progress cannot be followed from log file using "tail 

``level`` Attribute, ``/log:file,level=<Level>,...`` Option
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Log level is one of 'trace', 'debug', 'info' 'error', 'warning', 'critical'.

Default: 'info'.

``backtrace`` Attribute, ``/log:file,backtrace=<Level>,...`` Option
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Specify a log level which will trigger a log backtrace which will contain logs up to level 'debug'.

Value is one of 'trace', 'debug', 'info' 'error', 'warning', 'critical', off.

Default: 'error'.

``output`` Element, ``/log:file,output=Path>,...`` Option
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

*optional=yes, default=N/A*, `parent element: file`

Path to the log file. Patterns are supported as with archive element (cf `archive element <#the-archive-element>`_).

Example
--------

.. code:: xml

    <log>
        <console level="critical" backtrace="off"></console>
        <file level="error" backtrace="error">
            <output disposition="truncate">ORC_{SystemType}_{FullComputerName}_{TimeStamp}.dev.log</output>
        </file>
    </log>

.. code:: bat

    dfir-orc.exe /log:console,level=critical,backtrace=off /log:file,level=debug,backtrace=error,output="dfir-orc.log" ...

`Back to Root <#anchor-root>`_


``noconsole`` Attribute, ``/noconsole`` Option
-----------------------------------------------

This option disabled console output.

    .. code:: xml

        <logging noconsole=""/>

    ::

        /noconsole

``verbose`` Attribute, ``/verbose`` Option
------------------------------------------

Enables verbose output. **XML is deprecated**.

    .. code:: xml

        <logging verbose=""/>

    ::

        /verbose

``debug`` Attribute, ``/debug`` Option
--------------------------------------

Enables debug logging for Console and File log output. **XML is deprecated**.

    .. code:: xml

        <logging debug=""/>

    ::

        /debug

Example of debug logging:

.. code:: bat

    2021-02-08T17:43:41.200Z [I] WolfLauncher v10.1.0-rc3-115-ge4123652(orc.git 66613f2cdbc7fd9241eb9acabfab7a6ac19a242b


Typical Usage Example
=====================

.. code:: bat

    .\DFIR-Orc.exe NTFSInfo /noconsole /debug /log:file,level=error,output=c:\temp\ntfsinfo.log

This example does not output anything to the console (quiet mode), log information directly into an attached debugger and create "c:\\temp\\ntfsinfo.log" containing the console output.

The equivalent XML syntax is:

.. code:: xml

    <logging file="c:\temp\ntfsinfo.log" noconsole="" debug="" />