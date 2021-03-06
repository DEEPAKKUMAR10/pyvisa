.. _vpp43:


.. note:: This is a legacy module kept for backwards compatiblity with PyVISA < 1.5.
          and will be deprecated in future versions of PyVISA.
          You are strongly encouraged to switch to the new implementation.


About the legacy vpp43 module
=============================

This module ``vpp43`` is a cautious yet thorough adaption of the VISA
specification for Python.  The "textual languages" VISA specification can't be
implemented as is because Python is rather different from C and Visual Basic,
most notably because of lacking call-by-reference.  The second important
difference are strings: In C they are null-terminated whereas Python doesn't
have this constraint.

The slightly odd name ``vpp43`` for this module derives from the necessity to
make (name)space for the ``visa`` module that is supposed to realise the actual
high-level VISA access in Python.  The `VXIplug&play Systems Alliance`_ used to
maintain the VISA specifications, and, although today the `IVI foundation`_ is
responsible for this task, the files are still called ``vpp43.doc`` etc.  So I
thought ``vpp43`` was an appropriate name.

.. _`VXIplug&play Systems Alliance`: http://www.vxipnp.org/
.. _`IVI foundation`: http://ivifoundation.org

You may wonder why I did choose new names for all routines.  I did so because
Python has its own naming guidelines, and because it shows that the routines
had to be adapted.  However, I didn't change them really: Every routine is a
1:1 counterpart.  By calling them from C, you could even create a C-based VISA
implementation with the original function signatures and semantics.  Moreover,
the new names are mere expansions of the original ones.


Connecting to the VISA shared object
------------------------------------

``vpp43`` tries to find the VISA library for itself.  On Windows, this is not a
big problem.  ``visa32.dll`` must be in your ``PATH``.  If it isn't, move it
there or expand your ``PATH``.

However, on Linux you may need to give the explicit path to the shared object
file.  You do so by saying for example::

    from pyvisa.legacy import vpp43
    vpp43.visa_library.load_library("/path/to/my/libvisa.so.7")

By default, ``vpp43`` looks for the library in
``/usr/local/vxipnp/linux/bin/libvisa.so.7``.  Please pay attention to the fact
that the library must have been successfully loaded *before* any VISA call is
made.

Alternatively, you can tell PyVISA so by creating a file ``~/.pyvisarc``.  This
has the format of an INI file.  For example, if the library is at
``/usr/lib/libvisa.so.7``, the file ``.pyvisarc`` must contain the following::

    [Paths]

    VISA library: /usr/lib/libvisa.so.7

Please note that ``[Paths]`` is treated case-sensitively.

You can define a site-wide configuration file at
``/usr/share/pyvisa/.pyvisarc``.  (It may also be ``/usr/local/...`` depending
on the location of your Python.)


Diagnostics
-----------

This module can raise a couple of ``vpp43``-specific exceptions.

:Name: VisaIOError
:Description: This is an error of the underlying VISA library, as described in
    table 3.3.1 in the `VISA specification for textual languages`_.  The
    exception member ``error_code`` contains the (always negative) VISA error
    number, as listed in that table.

:Name: VisaIOWarning
:Description: This is a warning of the underlying VISA library, as described in
    table 3.3.1 in the `VISA specification for textual languages`_.  The
    exception member ``completion_code`` contains the (always positive) VISA
    completion number, as listed in that table.

    Normally you don't see these warnings.  You can turn them into exceptions
    with::

        import warnings
        warnings.filterwarnings("error")

    Consult the description of the ``warnings`` package for further
    information.

.. _`VISA specification for textual languages`:
       http://www.ivifoundation.org/Downloads/Class%20Specifications/vpp432.doc

:Name: TypeError
:Description: The current implementation of `printf`_, `scanf`_, `sprintf`_,
    `sscanf`_, and `queryf`_ have the limitation that only integers, floats,
    and strings are allowed as types for the arbitrary arguments.
    Additionally, only format string directives for C longs, C doubles, and C
    strings are allowed to use, albeit not checked.  However, if you pass a
    list or a unicode string, you get this exception.

    The same exception is raised if an unsupported type is passed as user
    handle to `install_handler`_.  See there for a list of supported types.

:Name: UnknownHandler
:Description: Raised if an unknown `handler`/`user_handle` pair is passed to
    `uninstall_handler`_.  In particular, you must save the user handle
    returned by `install_handler`_ in order to pass it to uninstall_handler.

Moreover, this module may pass exceptions generated by ctypes.  This may be
because you've passed a wrong type to a function, or that the VISA library file
was not found, but it may also mean a bug in ``vpp43`` itself.  So if you don't
see why the exception was raised, contact the current maintainers of PyVISA.





..  LocalWords:  rst british vpp PyVISA ies dll Gregor Thalhammer ViSession atn
..  LocalWords:  viAssertIntrSignal viAssertTrigger PROT viAssertUtilSignal ren
..  LocalWords:  viBufRead viBufWrite viClear viClose ViEvent ViFindList HNDLR
..  LocalWords:  viDisableEvent viDiscardEvents viEnableEvent viFindNext gpib
..  LocalWords:  viFindRsrc viFlush viGetAttribute viGpibCommand ADDR ifc viIn
..  LocalWords:  viGpibControlATN viGpibControlREN viGpibPassControl ViHndlr
..  LocalWords:  viGpibSendIFC viInstallHandler viLock viMapAddress ViAddr TMO
..  LocalWords:  ViBusAddress viMapTrigger viMemAlloc viMemFree viMove ViJobId
..  LocalWords:  viMoveAsync viMoveIn viMoveOut viOpen viOpenDefaultRM viOut
..  LocalWords:  viParseRsrc unaliased viParseRsrcEx INSTR viPeek vipoke printf
..  LocalWords:  scanf viPrintf queryf viQueryf viRead viReadAsync stb viScanf
..  LocalWords:  viReadSTB viReadToFile viSetAttribute viSetBuf sprintf sscanf
..  LocalWords:  viSPrintf viSScanf viStatusDesc viTerminate viUninstallHandler
..  LocalWords:  viUnlock viUnmapAddress viUnmapTrigger usb viUsbControlIn vxi
..  LocalWords:  viUsbControlOut vprintf vqueryf vscanf vsprintf vsscanf IVI
..  LocalWords:  viVxiCommandQuery viWaitOnEvent viWrite viWriteAsync WINNT def
..  LocalWords:  viWriteFromFile FixMe VisaIOError TypeError ctypes Enthought
..  LocalWords:  VisaIOWarning UnknownHandler pyvisa
