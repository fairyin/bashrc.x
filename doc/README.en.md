Bashrc.X
========

Enhanced [BASH][] Profiles
--------------------------

Rewritten to have the same experience, but 3~5 times more faster than
[ProfileX][].

INSTALL and UNINSTALL
---------------------

For most widely usage,
[GNU Makefile](http://www.gnu.org/software/make/manual/make.html), which used by
[ProfileX][], has been abandoned. Copy and then execute the following
corresponding codes, INSTALLation or UNINSTALLation would be done.

### INSTALL ###

```shell
'mkdir' -p ~/.local && cd ~/.local
[ -d bashrc.x.git/.git ] || \
  'rm' -fr bashrc.x.git && \
    'git' clone git://github.com/snakevil/bashrc.x bashrc.x.git
[ -L bashrc.x ] || 'rm' -fr bashrc.x && 'ln' -s bashrc.x.git/src bashrc.x
for i in bash_profile bashrc; do
  'rm' -fr ~/.$i
  'ln' -s .local/bashrc.x/etc/$i ~/.$i
done
cd -
```

### UNINSTALL ###

```shell
'rm' -fr ~/.bash_profile ~/.bashrc ~/.local/bashrc.x ~/.local/bashrc.x.git
```

Architecture Details
--------------------

### Execution Workflow ###

![Execution Workflow](https://raw.github.com/snakevil/bashrc.x/master/doc/workflow.png)

As the image shown, scripting files matched `[0-9][0-9]-*.sh` in the folder
`etc/bashrc.d` of `Bashrc.X` should be merged with the familar scripting files
in the folder `~/.bashrc.x/bashrc.d`, sorted by filenames, and then execute.

Notice that the default scripting files inside `Bashrc.x` have the higher
priority.

### PROMPTing Fields ###

Dynamic PROMPTing contents would be generated and merged by
[etc/bashrc.d/99-prompt.sh](https://github.com/snakevil/bashrc.x/blob/master/src/etc/bashrc.d/99-prompt.sh).

![PROMPTing Sample](https://raw.github.com/snakevil/bashrc.x/master/doc/prompting-sample.png)

* `Tue Nov 13` - **Date**
* <`hg` - **Effective user in `SUDO` mode**
* (`snakevil`) - **Login user**
* @`.94.32` - **Last 2 parts of `IP`**
* :`/h*/s*/Doc*/P*/ba*/s*/e*/bashrc.d` - **`CWD`_(compressed)_ **
* /`g` - **Repository type at `CWD`**
* `master`> - **Repository branch at `CWD`**
* `15:20` - **Time**
* j`1` - **Jobs in background**
* l`1.15`@2 - **System load (@ CPU cores)**
* m`73.0`% - **Memeory usage**
* c`0s54` - **Time consumed by last command**
* e`148` - **Exit code of last command**

Customization
-------------

### Features Configuration ###

To make `Bashrc.X` more agile, you can use the command `config-bashrc.x`. For
example:

* `prompt.date` - **Display / Suppress the date**
* `prompt.exit` - **Enable / Disable the last command exit code plugin**
* `prompt.ip.cut` - ** Enable / Disable the IP cut mode**
* `prompt.jobs` - **Enable / Disable background jobs plugin**
* `prompt.load` - **Enable / Disable the system load plugin**
* `prompt.load.cores` - **Display / Suppress the `CPU` cores of the system load
  plugin**
* `prompt.load.interval` - **Set the interval time (seconds) of updating system
  load**
* `prompt.mem` - **Enable / Disable the memory usage plugin**
* `prompt.mem.interval` - **Set the interval time (seconds) of updating memory
  usage**
* `prompt.pwd.compressed` - **Enable / Disable CWD compression mode**
* `prompt.time` - **Display / Suppress the date**
* `prompt.time-consumed` - **Enable / Disable the last command time-consuming
  plugin**
* `prompt.vcs` - **Enable / Disable the repository branch plugin**
* `prompt.vcs.delim` - **Set the heading content of repository branch plugin**

For more detailed description and help page of configuration variables, please
try `config-bashrc.x --help` and `config-bashrc.x -la`.

### PROMPTing Colors ###

Totally 18 colors variables, which are provided by
[etc/bashrc.d/05-colors.sh](https://github.com/snakevil/bashrc.x/blob/master/src/etc/bashrc.d/05-colors.sh),
are used for PROMPTing.

To re-color the PROMPTing as you like, please create `~/.bashrc.x/colors.rc`,
and defined corresponding variables.

Colors defeind by `Bashrc.X`:

* `$BASHRCX_COLOS[default]` - **Default color**, defaults as `$Chblack`
* `$BASHRCX_COLOS[user]` - **User color**, defaults as `$Ccyan`
* `$BASHRCX_COLOS[user.sudo]` - **Effective user color in `SUDO` mode**, defaults as `$Cred`
* `$BASHRCX_COLOS[ip]` - **`IP` color**, defaults as `$Cpurple`
* `$BASHRCX_COLOS[ip.ssh]` - **`IP` under `SSH` color**, defaults as
  `$Chblue`
* `$BASHRCX_COLOS[pwd]` - **`CWD` color**, defaults as `$Cyellow`
* `$BASHRCX_COLOS[vcs]` - **Repository branch color at `CWD`**, defaults as `$Cgreen`
* `$BASHRCX_COLOS[jobs]` - **Background jobs color**, defaults as `$Chwhite`
* `$BASHRCX_COLOS[load]` - **Load default color**, defaults as `$Cgreen`
* `$BASHRCX_COLOS[load.2]` - **Load color as notice (0.10-1.00)**, defaults as `$Cyellow`
* `$BASHRCX_COLOS[load.3]` - **Load color as warning (1.00+)**, defaults as `$Cred`
* `$BASHRCX_COLOS[mem]` - **Memory default color**, defaults as `$Cgreen`
* `$BASHRCX_COLOS[mem.2]` - **Memory color as notice (40.0-79.9%)**, defaults as `$Cyellow`
* `$BASHRCX_COLOS[mem.3]` - **memory color as warning (80.0%+)**, defaults as `$Cred`
* `$BASHRCX_COLOS[time-consumed]` - **Time consumed color by last command**,
  defaults as `$Cwhite`
* `$BASHRCX_COLOS[exit]` - **Exit code color of last command**, defaults as
  `$Chred`

### Extensions ###

What you need is to create scripting files named alike `[0-9][0-9]-*.sh` in the
folder `~/.bashrc.x/bashrc.d`. And then, the scripts should be executed
everytime you open the terminal.

### PROMPTing Plugins ###

PROMPTing plugins are a special kind of extensions in `Bashrc.X`. For most
perfect experience, we should know the following 2 instructions:

#### 1. Where could plugins display? ####

![Plugins Positions](https://raw.github.com/snakevil/bashrc.x/master/doc/plugins-positions.png)

#### 2. How plugins declared? ####

1. Filename

    For most compatibility, we strongly recommend the filename should match the
    rule `9[0-8]-prompt-*.sh`.

2. Function

    Function name should be named alike `_bashrc.x-prompt-[1-2].[0-9][0-9]-*`.
    Give it a more unique name, for all declared plugins could work fine.

3. Return value

    For functions in [BASH][] could not return values directly, we chosen the
    special array variable `_pret` to act as the return parameter.

    ```shell
    _pret=(
      1  # Controls the display position. '1' or '2' available, for the position
         # after CWD, or before the prompting symbol.
      "" # Controls the content including colors.
    )
    ```

For more information, please read the sample:
[etc/bashrc.d/90-prompt-jobs.sh](https://github.com/snakevil/bashrc.x/blob/master/src/etc/bashrc.d/90-prompt-jobs.sh).

###### Copyright © 2012 [Snakevil Zen][me]. ALL RIGHTS RESERVED. ######

[profilex]: https://github.com/snakevil/profilex (ProfileX)
[bash]: http://www.gnu.org/software/bash/manual/html_node/index.html
[me]: https://szen.in
