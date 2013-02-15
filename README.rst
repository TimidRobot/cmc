cmc
===

``cmc`` makes SSH ControlMaster sessions easy. SSH ControlMaster sessions have
the following benefits:

- When using a SSH Bastion, you will only be prompted for (two-factor)
  authentication once.

  - Hosts behind the SSH Bastion can be accessed "directly" by proxying through
    the SSH Bastion (see workflow_).

- Sessions to or through ControlMaster hosts do not need to create a new
  connection (most people will enjoy faster SSH)

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
        -l      list ControlMaster connection sockets in ~/.ssh/ and check their
                connection status
        -O      open all ControlMasters defined in SSH_CONFIG
        -o HOST open a ControlMaster session (maybe specified more than once)
        -x HOST close ControlMaster session (maybe specified more than once)
        -X      exit all ControlMaster connections with sockets in ~/.ssh/

    Notes:
        * Any unopened sockets in ~/.ssh/ are removed with -l and -X


Workflow
========

**See** |workflow|_

.. |workflow| replace:: **docs/workflow.rst**
.. _workflow: docs/workflow.rst


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

- \*nix Operating System with the GNU Bourne-Again SHell (``bash``) and core
  utilities:

  - ``awk``
  - ``find``
  - ``grep``
  - ``sed``
  - ``ssh`` (OpenSSH 4.0 or later)


License
=======

- LICENSE_ (`MIT License`_)

.. _LICENSE: LICENSE
.. _`MIT License`: http://www.opensource.org/licenses/MIT
