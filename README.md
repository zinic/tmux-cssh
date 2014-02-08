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

## User-Home-Settings-file ~/.tmux-cssh

This file is located in the called user home directory and includes predefined parameters in a single line.

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
