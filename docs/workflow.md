# Workflow

SSH is a powerful tool. When configured correctly it should not only provide
excellent security, it should also make your work *easier* and
*more convenient*.


## Assumptions

1. You're using macOS
   - This is not a requirement. The `cmc` script should work on any \*nix.
2. You have already created a SSH key pair and added the secret key to your
   Mac keychain.
   - Test to see currently available keys:
     1. `ssh-add -L`
   - SSH key pair creation example:
     1. `ssh-keygen -b 4096 -C USERNAME@COMPUTER_DESC`
     2. `ssh-add -K`
3. Your name is Arthur Dent
   - Your username on your Mac laptop is `arthurdent`
   - Your username on remote systems is `arthur`
4. Only the `~/.ssh/config` on your laptop will ever need to be edited.
5. Three hypothetical hosts (see below)


### Example `~/.ssh/config` Configuration

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

Please also see the annotated ssh configuration example with explanations:
[`annotated_config.md`](annotated_config.md).


## Workflow

1. Connect "directly" to hosts behind the firewall using bastion as a proxy
   - `ssh prod`
   - Automatically connects to `bastion` and proxies through it.
2. Realize you need to completely reconnect for some reason (ex. you made an
   error in your gpg-agent configuration).
3. List current connections with `cmc -l`.
4. Close impacted connections with
   - `cmc -x bastion` (which will automatically close the connection to `prod`)
   - or `cmc -X` to close all active connections


## References

- **[ssh_config(5)][mansshconfig]**

[mansshconfig]:http://man.openbsd.org/OpenBSD-current/man5/ssh_config.5
