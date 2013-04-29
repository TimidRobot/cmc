********
Workflow
********

SSH is a powerful tool. When configured correctly it should not only provide
excellent security, it should also make your work *easier* and
*more convenient*.


Assumptions
===========
1. You're using Mac OS X

   - This is not a requirement. The ``cmc`` script should work on any \*nix.

2. You have already created a SSH key pair and added the secret key to your
   Mac keychain.

   - Test to see currently available keys:

     A. ``ssh-add -L``

   - SSH key pair creation example:

     A. ``ssh-keygen -b 4096 -C USERNAME@COMPUTER_DESC``
     B. ``ssh-add -K``

3. Your name is Arthur Dent

   - Your username on your Mac laptop is ``arthurdent``
   - Your username on remote systems is ``arthur``

4. Only the ``~/.ssh/config`` on your laptop will ever need to be edited.

5. Three hypothetical hosts (see below)


Example ``~/.ssh/config`` Configuration
---------------------------------------

::

    # insecure
    Host insecure insecure.example.com
        HostName insecure.example.com

    # bastion
    Host bastion bastion.example.com
        HostName bastion.example.com
        ForwardAgent yes
        ControlMaster auto

    # production
    Host prod production prod*.example.com
        HostName production.example.com
        ForwardAgent yes
        ProxyCommand ssh -q bastion nc -w30 %h %p

    # global defaults
    Host *
        ControlPath ~/.ssh/master-%r@%h:%p
        ServerAliveCountMax 6
        ServerAliveInterval 10
        User arthur


Please also see the `annotated ssh configuration example`_ with explanations.

.. _`annotated ssh configuration example`: annotated_config.rst


Workflow
========

1. Establish control sessions at the start of your day/session/etc.

   - ``cmc bastion``
   - This establishes a control master connection in the background. It will
     stay connected and available until it is closed or connectivity is lost.

2. Connect "directly" to hosts behind the firewall using bastion as a proxy

   - ``ssh prod``
   - Uses the connection already in place when it proxies through bastion!


References
==========

- `ssh_config(5) OS X Manual Page`_

.. _`ssh_config(5) OS X Manual Page`:
   https://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man5/ssh_config.5.html
