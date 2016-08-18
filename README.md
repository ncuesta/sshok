# `sshok` is a transparent SSH configuration management tool

Sick of your hundreds-lines-long `.ssh/config` file? This is *the tool* for you!

`sshok` is a simple `ssh` command wrapper that allows you to split your SSH configuration file into smaller files living in a configuration directory (`$HOME/.ssh/config.d/` by default, although this is customizable) and seamlessly keeps your main SSH configuration file (`$HOME/.ssh/config` by default, also customizable) in sync with those files.

The idea is simple: instead of having a single, humongous configuration file, split it into smaller, focused, well-organized files into a single directory and have `sshok` merge them into the file that `ssh` reads.

## Installation

Just [download the latest version of the script](https://raw.githubusercontent.com/ncuesta/sshok/master/sshok) and place it somewhere inside your `$PATH`. Then off you go!

Or copy & paste this (if you dare -- and have the required permissions):

```console
# This will download the latest version of sshok to /usr/local/bin and grant exec permission to all users on that file
$ curl https://raw.githubusercontent.com/ncuesta/sshok/master/sshok > /usr/local/bin/sshok && chmod a+x /usr/local/bin/sshok
```

> Note that for the above snippet to work, `/usr/local/bin` needs to be inside your `$PATH` environment variable.

## Usage

`sshok` is intended to be transparent, so you should be able to run any `ssh` command using `sshok` just by replacing the former command with the latter in your script/commands line. For example:

```console
# Old, gross way
$ ssh user@host.example.org
# New, shiny way
$ sshok user@host.example.org
```

So... what's the point then? Just making your local environment a safer and nicer place to work at. By splitting your SSH configuration into many smaller files you can organize them better and have much more visibility on what you have configured.

Oh, and every time `sshok` needs to change the configuration file it will gently back the current one both alongside the newly-generated one and to a customizable backups directory. For more info on that, you may want to check out the source. Have you seen those lovely comments?

## Using an alias

You can define an alias for `sshok` so that instead of those two productivity draining keypresses (<kbd>o</kbd> and <kbd>k</kbd>) like follows:

```console
$ alias ssh=sshok
```

Just put that in your shells `rc` file, logout/login again and bam! Just use `ssh` normally to run `sshok`.

## Getting completion to work

If, like me, you depend on your <kbd>Tab</kbd> key to the point of not remembering FQDNs or even host aliases, you're going to need to enable shell completion for `sshok`. How exactly you do that, depends on your shell.

### For zsh (using oh-my-zsh, antigen or similar)

You will need to put this in your `.zshrc` file:

```shell
compdef '_dispatch ssh ssh' sshok
```

## Customization

`sshok` can be customized by specifying the following environment variables:

| Variable name             | Default value       | Description                                                                                                                                       |
| ------------------------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$SSH_DIR`                | `$HOME/.ssh`        | Path to the user's SSH directory.                                                                                                                 |
| `$SSH_CONFIG_DIR`         | `$SSH_DIR/config.d` | Path to where the smaller configuration files are located.                                                                                        |
| `$SSH_CONFIG_FILE`        | `SSH_DIR/config`    | Path to the configuration file used by `ssh`.                                                                                                     |
| `$BACKUPS_DIR`            | `/tmp/sshok`        | Path to the directory where configuration backups will be stored. A backup will be created every time the `$SSH_CONFIG_FILE` needs to be updated. |
| `$UNWANTED_LINES_PATTERN` | `"# vim:"`          | String that will be fed to `grep -v` to filter out unwanted lines when concatenating the files.                                                   |
| `$SSH_CONFIG_DIRS`        | `$SSH_CONFIG_DIR`   | String with a space-delimited list of directories to look for source configuration files and where to put the each resulting concatenated file.   |

These may be overridden by passing the as local variables for `sshok` or by having them defined as global `ENV` variables:

```console
$ BACKUPS_DIR=/tmp/my-other-storage-is-replicated sshok user@host
# or
$ export BACKUPS_DIR=/tmp/my-other-storage-is-replicated
$ sshok user@host
```

## Migration

Just by running `sshok`, if the `$SSH_CONFIG_DIR` does not exist it will be created and the contents of `$SSH_CONFIG_FILE` will be copied to `$SSH_CONFIG_DIR/original` so that no information is lost.

## Contributors

Thanks to @leoditommaso for pushing me into this. :laughing:

Thanks to @einar-lanfranco for working on #1.

## License

`sshok` is licensed under the MIT license. Legal `char *` follows:


```
MIT License

Copyright (c) 2016 Nahuel Cuesta Luengo

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
