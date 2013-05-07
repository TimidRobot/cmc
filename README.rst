cmc
===

``cmc`` makes SSH ControlMaster sessions easy. SSH ControlMaster sessions have
the following benefits:

- When using a SSH Bastion, you will only be prompted for (two-factor)
  authentication once.

  - Hosts behind the SSH Bastion can be accessed "directly" by proxying through
    the SSH Bastion (see workflow_).

- Sessions to or through ControlMaster hosts do not need to create a new
  connection (SSH will be faster for most tasks)

Script Help
-----------

::

    Usage:  cmc [ -c HOST | -o HOST | -x HOST ]
            cmc [ -L | -l | -O | -X ]
            cmc -h

    ControlMaster Controller - Eases management of SSH ControlMaster connections

    Options:
        -h      show this help message and exit
        -c HOST check HOST ControlMaster connection status (maybe specified more
                than once)
        -L      list ControlMasters defined in SSH_CONFIG
        -l      list ControlMaster connection sockets matching ControlPath and
                check their connection status
        -O      open all ControlMasters defined in SSH_CONFIG
        -o HOST open a ControlMaster session (maybe specified more than once)
        -x HOST exit ControlMaster session (maybe specified more than once)
        -X      exit all ControlMaster connections with sockets matching
                ControlPath

    Notes:
        • Any unused sockets in ControlPath are removed with -l and -X
        • Only a single ControlPath should be specified


Install
=======

Download ``cmc`` and ensure it is in your ``PATH``.


Documentation
=============

Workflow
--------

**See** |workflow|_

.. |workflow| replace:: **docs/workflow.rst**
.. _workflow: docs/workflow.rst

Annotated SSH Configuration
---------------------------

**See** |annotated_config|_

.. |annotated_config| replace:: **docs/annotated_config.rst**
.. _annotated_config: docs/annotated_config.rst


Related
=======

* mac-ssh-confirm_: Protect against SSH Agent Hijacking on Mac OS X with the
  ability to confirm agent identities prior to each use
* gacli_: Mac CLI Google Authenticator client (ex. for use with SSH Bastions
  that utilize Google Authenticator)

.. _mac-ssh-confirm: https://github.com/TimZehta/mac-ssh-confirm
.. _gacli: https://github.com/ClockworkNet/gacli


Requirements
============

- \*nix Operating System with

  - core utilities (``awk``, ``find``, ``grep``, ``ps``, and ``sed``)
  - GNU Bourne-Again Shell 3.0 or later (``bash``)
  - OpenSSH 4.0 or later (``ssh``)


License
=======

- LICENSE_ (`MIT License`_)

.. _LICENSE: LICENSE
.. _`MIT License`: http://www.opensource.org/licenses/MIT
