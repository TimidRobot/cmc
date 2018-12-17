# Annotated SSH Configuration

Please also see the workflow documentation: [`workflow.md`](workflow.md).


## Example SSH Config

```
# insecure
Host insecure insecure.example.com
    HostName insecure.example.com

# bastion
Host bastion bastion.example.com
    HostName bastion.example.com
    ControlPersist 8h

# production
Host prod production prod*.example.com
    HostName production.example.com
    ControlPersist 2h
    ProxyCommand ssh -q bastion nc -w30 %h %p

# global defaults
Host *
    ControlMaster auto
    ControlPath ~/.ssh/cp_%r_%h
    ControlPersist 5m
    ServerAliveCountMax 60
    ServerAliveInterval 30
    TCPKeepAlive no
    User arthur
```


### Section: `# insecure`

This section is for a server on the Internet that we think is insecure (we
do not trust the administrators--those with root access).

```
# insecure
Host insecure insecure.example.com
    HostName insecure.example.com
```

1. `# insecure` is a comment. It helps provide context for for the
   line that follows it.
2. `Host insecure insecure.example.com` indicates the host patterns that the
   subsequent parameters apply to. All of the following will work to connect
   to the configured HostName:
   - `ssh insecure`
   - `ssh insecure.example.com`
3. `HostName insecure.example.com` specifies the real host name to log into.


### Section: `# bastion`

This section is for a server on the Internet that acts as a SSH bastion. It
provides access to servers behind a firewall. ::

```
# bastion
Host bastion bastion.example.com
    HostName bastion.example.com
    ControlPersist 8h
```

1. `# bastion` is a comment. It helps provide context for for the
   line that follows it.
2. `Host bastion bastion.example.com` indicates the host patterns that the
   subsequent parameters apply to. All of the following will work to connect
   to the configured HostName:
   - `ssh bastion`
   - `ssh bastion.example.com`
3. `HostName bastion.example.com` specifies the real host name to log into.
4. `ControlPersist 8h` specifies that the master connection should remain open
   and idle in the background for up to 8 hours. This is especially convenient
   when the bastion server requires Multifactor authentication (MFA).
    - `ControlPersist` is available as of OpenSSH 5.6. For previous versions,
      simply omit it.


### Section: `# production`

This section is for a server on the Internet that acts as a SSH production. It
provides access to servers behind a firewall.

```
# production
Host prod production prod*.example.com
    HostName production.example.com
    ControlPersist 2h
    ProxyJump bastion
```

1. `# production` is a comment. It helps provide context for for the
   line that follows it.
2. `Host prod production prod*.example.com` indicates the host patterns that
   the subsequent parameters apply to. All of the following will work to
   connect to the configured HostName:
   - `ssh prod`
   - `ssh production`
   - `ssh prod.example.com`
   - `ssh production.example.com`
3. `HostName production.example.com` specifies the real host name to log into.
4. `ControlPersist 2h` specifies that the master connection should remain open
   and idle in the background for up to 2 hours.
    - `ControlPersist` is available as of OpenSSH 5.6. For previous versions,
      simply omit it.
5. `ProxyJump bastion` specifies that SSH host to proxy connections through.
    Any SSH client (ex. ssh command line, git, Transmit app) will see the
    production session as a single connection. It just works!
    - `ProxyJump` is available as of OpenSSH 7.3
      - For OpenSSH versions 5.4 through 7.2 use:
        ```
        ProxyCommand ssh bastion -W %h:%p
        ```
      - For OpenSSH versions 5.3 and below use:
        ```
        ProxyCommand ssh -q bastion nc -w30 %h %pi
        ```


### Section: `# global defaults`

The global defaults for all hosts is specified last. Its parameters apply if
they are not previously defined (which is why it should be the *last* section
of your SSH config).

```
# global defaults
Host *
    ControlMaster auto
    ControlPath ~/.ssh/cp_%r_%h
    ControlPersist 5m
    ServerAliveCountMax 60
    ServerAliveInterval 30
    TCPKeepAlive no
    User arthur
```

1. `# global defaults` is a comment. It helps provide context for for the
   line that follows it.
2. `Host *` indicates this is the global defaults section.
3. `ControlPath ~/.ssh/cp_%r_%h` supports the ControlMaster parameter. The
    path given here supports longer host names which can otherwise cause
    issues.
4. `ControlPersist 5m` specifies that the master connection should remain open
   and idle in the background for up to 5 minutes. This will speedup version
   control commands while also being a good conservative default.
    - `ControlPersist` is available as of OpenSSH 5.6. For previous versions,
      simply omit it.
5. `ServerAliveCountMax 60` helps ensure robust proxied sessions.
6. `ServerAliveInterval 30`  helps ensure robust proxied sessions.
    - A `ServerAliveInterval` of 30s combined with a `ServerAliveCountMax` of
      60 will result in disconnections of unresponsive clients after half an
      hour.
    - The relatively short `ClientAliveInterval` should ensure aggressive TTLs
      do not severe connections. The larger `ClientAliveCountMax` should allow
      brief interruptions without disrupting work.
7. `TCPKeepAlive no` allows connections to weather short network outages
   (especially useful when connected via WiFi).
8. `User arthur` specifies the user to log in as (remember, in our example
   the local username is arthurdent).

Additionally, the following defaults are important. The parameter is not in
this section because the OpenSSH default value is appropriate. It should be
acknowledged so that it is not unintentionally superseded by a configured
parameter:

- `ForwardAgent no` specifies that the authentication agent will **not** be
  forwarded. This prevents administrators on untrusted remote servers from
  masquerading as you on *any* system on which you have your SSH public key.
  See [SSH Agent Hijacking][hijacking] for more information.


## References

- **[ssh_config(5)][mansshconfig]**
- [SSH Agent Hijacking][hijacking]

[mansshconfig]:http://man.openbsd.org/OpenBSD-current/man5/ssh_config.5
[hijacking]:http://www.clockwork.net/blog/2012/09/28/602/ssh_agent_hijacking
