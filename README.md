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
Usage:  cmc [ -c HOST | -x HOST ]
        cmc [ -l | -X ]
        cmc -h

ControlMaster Controller - Eases management of SSH ControlMaster connections

Options:
    -h      show this help message and exit
    -c HOST check HOST ControlMaster connection status (may be specified more
            than once)
    -d      print debug information
    -l      list all active ControlMaster connection sockets
    -x HOST exit ControlMaster session (may be specified more than once)
    -X      exit all ControlMaster connections with sockets

Notes:
    - Any unused sockets in ControlPath are removed with -l and -X
```
(output of `cmc -h`)


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

[brewinstall]: http://brew.sh/#install


## Documentation


### ControlPersist

Prior to the addition of `ControlPersist` in [OpenSSH 5.6][openssh56], this
utility was needed. However it still convenient to be able to quickly manage
ControlMaster connections.

For more information on `ControlPersist` see [ssh_config(5)][mansshconfig].

[openssh56]: https://www.openssh.com/txt/release-5.6
[mansshconfig]: http://man.openbsd.org/OpenBSD-current/man5/ssh_config.5


### Workflow

See [`docs/workflow`][workflow].

[workflow]: docs/workflow.md


### Annotated SSH Configuration

See [`docs/annotated_config`][annotated].

[annotated]: docs/annotated_config.md


### Related

- any \*nix Operating Systems (including Linux and macOS):
  - **[solo-agent][soloagent]**: Enable discrete SSH Agents to avoid leaking
    access across hosts 
- macOS only
  - [mac-ssh-confirm][confirm]: Protect against SSH Agent Hijacking on macOS
    with the ability to confirm agent identities prior to each use

[gacli]: https://github.com/TimidRobot/gacli
[soloagent]: https://github.com/TimidRobot/solo-agent
[confirm]: https://github.com/TimidRobot/mac-ssh-confirm


## Requirements

- any \*nix Operating System (including Linux and macOS) with:
  - core utilities (`awk`, `find`, `grep`, `ps`, and `sed`)
  - GNU Bourne-Again Shell 3.0 or later (`bash`)
  - OpenSSH 5.6 or later (`ssh`)
    - For OpenSSH versions between 4.0 and 5.6, try [cmc 1.0.3][cmc103]

[cmc103]:https://github.com/TimidRobot/cmc/tree/1.0.3


## Development

Run `./testcmc TESTHOST` prior to signing a new release.

Thank you:
- [shellcheck][shellcheck] - a static analysis tool for shell

[shellcheck]: https://github.com/koalaman/shellcheck


## License

- [`LICENSE`](LICENSE) (Expat/[MIT][mit] License)

[mit]: http://www.opensource.org/licenses/MIT "The MIT License | Open Source Initiative"
