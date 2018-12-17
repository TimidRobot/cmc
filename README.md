# cmc

ControlMaster Controller - Eases management of SSH ControlMaster connections


## Overview

`cmc` makes SSH ControlMaster sessions easy. SSH ControlMaster sessions have
the following benefits:
- When using a SSH Bastion, you will only be prompted for (two-factor)
  authentication once.
  - Hosts behind the SSH Bastion can be accessed "directly" by proxying through
    the SSH Bastion (see workflow_).
- Sessions to or through ControlMaster hosts do not need to create a new
  connection (SSH will be faster for most tasks)


## Script Help

```
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
```


## Install

1. [Install Homebrew][brewinstall] -- The missing package manager for macOS
2. Add the "tap":
    ```shell
    brew tap TimidRobot/tap
    ```
3. Install `cmc`:
    ```
    brew install cmc
    ```

Alternatively, since `cmc` is a bash script without esoteric dependencies,
you can simply download it and ensure it is in your `PATH`.

If this utility is helpful for you, please star this project so that it can
eventually be included in Homebrew proper. Thank you!

[brewinstall]:http://brew.sh/#install


## Documentation


### ControlPersist

Prior to the addition of `ControlPersist` in [OpenSSH 5.6][openssh56], this
utility was needed. However it still convenient to be able to quickly manage
ControlMaster connections.

For more information on `ControlPersist` see [ssh_config(5)][mansshconfig].

[openssh56]:https://www.openssh.com/txt/release-5.6
[mansshconfig]:http://man.openbsd.org/OpenBSD-current/man5/ssh_config.5


### Workflow

See [`docs/workflow`][workflow].

[workflow]:docs/workflow.rst


### Annotated SSH Configuration

See [`docs/annotated_config`][annotated].

[annotated]:docs/annotated_config.rst


### Related

- [mac-ssh-confirm][confirm]: Protect against SSH Agent Hijacking on macOS with
  the ability to confirm agent identities prior to each use
- [gacli][gacli]: Mac CLI Google Authenticator client (ex. for use with SSH
  Bastions that utilize Google Authenticator or TOTP tokens)

[confirm]:https://github.com/TimZehta/mac-ssh-confirm
[gacli]:https://github.com/ClockworkNet/gacli


## Requirements

- \*nix Operating System with
  - core utilities (`awk`, `find`, `grep`, `ps`, and `sed`)
  - GNU Bourne-Again Shell 3.0 or later (`bash`)
  - OpenSSH 4.0 or later (`ssh`)


## Supported By

Portions of the development of this project (and the supporting tap) were
supported by [ClockworkNet][Clockwork]. Thank you!

[Clockwork]: https://github.com/ClockworkNet


## License

- [`LICENSE`](LICENSE) (Expat/[MIT][mit] License)

[mit]: http://www.opensource.org/licenses/MIT "The MIT License | Open Source Initiative"
