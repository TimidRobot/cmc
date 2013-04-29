***************************
Annotated SSH Configuration
***************************


Example SSH Config
==================

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


Section: # insecure
-------------------

This section is for a server on the Internet that we think is insecure (we
do not trust the administrators--those with root access). ::

    # insecure
    Host insecure insecure.example.com
        HostName insecure.example.com

1. ``# insecure`` is a comment. It helps provide context for for the
   line that follows it.
2. ``Host insecure insecure.example.com`` indicates the host patterns that the
   subsequent parameters apply to. All of the following will work to connect
   to the configured HostName:

   - ``ssh insecure``
   - ``ssh insecure.example.com``

3. ``HostName insecure.example.com`` specifies the real host name to log into.

Additionally, the following defaults are important. The parameter is not in
this section because the default value is appropriate. It should be
acknowledged so that it is not unintentionally superseded by a configured
parameter:

4. ``ForwardAgent no`` specifies that the authentication agent will **not** be
   forwarded. This prevents administrators on untrusted remote servers from
   masquerading as you on *any* system on which you have your SSH public key.
   See `SSH Agent Hijacking`_ for more information.


Section: # bastion
------------------

This section is for a server on the Internet that acts as a SSH bastion. It
provides access to servers behind a firewall. ::

    # bastion
    Host bastion bastion.example.com
        HostName bastion.example.com
        ControlMaster auto
        ControlPath ~/.ssh/master-%r@%h:%p

1. ``# bastion`` is a comment. It helps provide context for for the
   line that follows it.
2. ``Host bastion bastion.example.com`` indicates the host patterns that the
   subsequent paramters apply to. All of the following will work to connect
   to the configured HostName:

   - ``ssh bastion``
   - ``ssh bastion.example.com``

3. ``HostName bastion.example.com`` specifies the real host name to log into.
4. ``ForwardAgent yes`` specifies that the authentication agent will be
   forwarded to the remote server.

   - This is important for the bastion server as it allows public key sessions
     from the bastion to other servers (especially those behind the firewall).
     This means you will be able to connect to those servers without a
     password.

5. ``ControlMaster auto`` indicates SSH should listen for connections on a
   control socket. Additional sessions can connect to this socket and reuse
   the master instances (bastion's) network connection rather than initiating
   a new one.


Section: # production
---------------------

This section is for a server on the Internet that acts as a SSH production. It
provides access to servers behind a firewall.

::

    # production
    Host prod production prod*.example.com
        HostName production.example.com
        ForwardAgent yes
        ProxyCommand ssh -q bastion nc -w30 %h %p

1. ``# production`` is a comment. It helps provide context for for the
   line that follows it.
2. ``Host prod production prod*.example.com`` indicates the host patterns that
   the subsequent parameters apply to. All of the following will work to connect
   to the configured HostName:

   - ``ssh prod``
   - ``ssh production``
   - ``ssh prod.example.com``
   - ``ssh production.example.com``

3. ``HostName production.example.com`` specifies the real host name to log into.
4. ``ForwardAgent yes`` specifies that the authentication agent will be
   forwarded to the remote server.

   - This is important for the production server as it allows public key
     sessions from the production server to other servers (especially source
     code repository servers).

5. ``ProxyCommand ssh -q bastion nc -w30 %h %p`` specifies the command to use
   to connect to the server.

   - This allows the connections to servers behind the firewall using the
     bastion server as a proxy. Any SSH client (ex. ssh command line, svn,
     Transmit) will see the production session as a single connection. It
     just works!


Section: # global defaults
--------------------------

The global defaults for all hosts is specified last. Its parameters apply if
they are not previously defined (which is why it should be the *last* section
of your SSH config). ::

    # global defaults
    Host *
        User arthur
        ForwardAgent no
        ServerAliveCountMax 6
        ServerAliveInterval 10

1. ``# global defaults`` is a comment. It helps provide context for for the
   line that follows it.
2. ``Host *`` indicates this is the global defaults section.
3. ``ControlPath ~/.ssh/master-%r@%h:%p`` supports the ControlMaster parameter.
   See `ssh_config(5) OS X Manual Page`_ if you are really curious.
4. ``ServerAliveCountMax 6`` helps ensure robust proxied sessions. See
   `ssh_config(5) OS X Manual Page`_ if you are really curious.
5. ``ServerAliveInterval 10``  helps ensure robust proxied sessions. See
   `ssh_config(5) OS X Manual Page`_ if you are really curious.
6. ``User arthur`` specifies the user to log in as (remember, in our example
   the local username is arthurdent).

Additionally, the following defaults are important. The parameter is not in
this section because the default value is appropriate. It should be
acknowledged so that it is not unintentionally superseded by a configured
parameter:

7. ``ForwardAgent no`` specifies that the authentication agent will **not** be
   forwarded. This prevents administrators on untrusted remote servers from
   masquerading as you on *any* system on which you have your SSH public key.
   See `SSH Agent Hijacking`_ for more information.


References
==========

- `ssh_config(5) OS X Manual Page`_
- `SSH Agent Hijacking`_

.. _`ssh_config(5) OS X Manual Page`:
   https://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man5/ssh_config.5.html
.. _SSH Agent Hijacking:
   http://www.clockwork.net/blog/2012/09/28/602/ssh_agent_hijacking
