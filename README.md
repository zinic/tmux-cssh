# tmux-cssh
_TMUX-C(luster)-SSH_

## Description

_tmux_ is a _terminal multiplexer_, like e.g. _screen_, which gives you a possibility to use multiple virtual terminal session within one real terminal session. _tmux-cssh_ (tmux-cluster-ssh) sets a comfortable and easy to use functionality, clustering and synchronizing virtual _tmux_-sessions, on top of _tmux_. No need for a x-server or x-forwarding. _tmux-cssh_ works just with _tmux_ and in an low-level terminal-environment, like most server do.

## Dependencies / Installation

```
$ apt-cache search --names-only tmux
tmux - Terminal-Multiplexer
```

... under _debian_-based systems:
```
$ sudo apt-get install tmux
```

## Deb-Packages

Take a look at the pre-build [deb-packages](https://github.com/dennishafemann/tmux-cssh/tree/deb-package/deb-packages).

## Usage / Example

First, take a look at the help- and syntax-texts:
```
$ tmux-cssh --help
```

You can connect to a single server, with a single connection-data:

```
$ tmux-cssh -sc my-user-name@my-own-server
```

You can connect multiple server, with different connection-data:
```
$ tmux-cssh -sc my-user-name@my-own-server -sc second_user@second_server
```

You can connect to multiple server, with a single connection-data:
```
$ tmux-cssh -u my-user-name -sc my-own-server -sc second_server
```

You can connect to multiple server, the short way:
```
$ tmux-cssh -u my-user-name my_server 1.2.3.4 11.22.33.44 my_second_server my_third_server my_and_so_on_server
```

You can load predefined parameters settings from your user-home-settings-file ~/.tmux-cssh.
```
$ tmux-cssh -cs [dev\|test\|productive]_servers
```

If you want to just open your multiple server in a grid, but don't want every keystrokes shared around you can add the parameter for "don't synchronize panes".
```
$ tmux-cssh -cs [dev\|test\|productive]_servers -ds
```

It is possible to use different tmux layouts, default is tiled:
```
$ tmux-cssh -tl even-vertical -u my-user-name my_server 1.2.3.4 11.22.33.44 my_second_server my_third_server my_and_so_on_server
```


## User-Home-Settings-file ~/.tmux-cssh

This file is located in the user home directory, from _tmux-cssh_-calling user, and includes predefined parameters in a single line.

```
dev_servers:-sc 10.10.1.1
test_servers:-sc 10.20.1.1 -sc 10.20.1.2
productive_servers:-sc 10.30.1.1 -sc 10.30.1.2 -sc 10.30.1.3
```

Each line can be analysed into to values _key_ and _parameters_, seperated by a colon (:).

`[key]:[parameters]`

Using the parameters _-cs|--config-setting_ the _key_ name is search via a single string or a (grep valid) regular expression.

```
$ tmux-cssh -cs [dev\|test\|productive]_servers
... or ...
$ tmux-cssh -cs server[1-9]
```

### Multiple -cs

With support of multiple -cs parameters you can now be more flexible.

```
clients:-sc 1.1.1.1 -sc 1.1.1.2
servers:-sc 10.10.10.1 -sc 10.10.10.2
all:-cs clients -cs servers
```

```
$ tmux-cssh -cs clients
$ tmux-cssh -cs servers
$ tmux-cssh -cs clients -cs servers
$ tmux-cssh -cs all
```

## Combine parameters

_tmux-cssh_ adds all given parameters to it's environment before calling the final _tmux_-session. So that means all parameters can be combined in each way.

**Call**

```
$ tmux-cssh -f /tmp/temp-server-hosts -cs "fixed_dev_server\|auth_user"
```

**Host file -f /tmp/temp-server-hosts**

```
10.10.1.1
dev_server_1
```

**Config-Settings name fix_dev_server in ~/.tmux-cssh**

```
auth_user:-u me_as_an_auth_user
fix_dev_server:10.10.1.10 10.10.1.20 10.10.1.30
```

**Finally tmux-cssh works like if you called ...**

```
$ tmux-cssh -u me_as_an_auth_user -sc 10.10.1.1 -sc dev_server_1 -sc 10.10.1.10 -sc 10.10.1.20 -sc 10.10.1.30
```

## TMUX-CSSH-GUI

There is a simple, useful and comfortable GUI set on top of tmux-cssh: TMUX-CSSH-GUI

<a href="https://github.com/dennishafemann/tmux-cssh-gui">TMUX-CSSH-GUI @ github</a>
