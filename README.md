[![Join the chat at https://gitter.im/mattoufoutu/passtis](https://badges.gitter.im/mattoufoutu/passtis.svg)](https://gitter.im/mattoufoutu/passtis?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

# Passtis

GnuPG-based command line password manager, greatly inspired by [pass](https://www.passwordstore.org/).


## Why?

Because I liked [pass](https://www.passwordstore.org/) idea of a GnuPG-based password vault, but I
was frustrated by its very simplistic storage format with which it's impossible to store metadata
along with passwords in a simple usable way.


## ToDo

* check if clipboard is available
* find a workaround for when clipboard is not available
* support arbitrary fields
* support configuration file (for example, to store custom password complexity settings)
* autocompletion scripts for bash/zsh
* create helpers for migrating from various password stores
* allow to encrypt entries with multiple keys
* allow to set specific keys to subfolders
* implement TOTP with encrypted key storage [IDEA]


## Installation


### Dependencies

* Python >= 2.6
* python-gnupg
* pyperclip
* argparse


### Installing manually

Make sure you've got all the dependencies installed, and copy the `passtis.py` script somewhere 
in your `PATH`.


### Installing using `setup.py`


    $ sudo python setup.py install


### Installing using `pip`


TODO


## Usage

* Initialize the password store

```
$ passtis init 13E22663
New store created: /home/user/.passtis-store
```

* Add a new entry (password input)

```
$ passtis add -u user -U http://example.com -c 'very password, much secure' example.com
Password: 
Confirm password:
```

* Add a new entry (generated password)

```
$ passtis add -u user -U https://github.com github.com --generate
new password copied to clipboard (will be cleared in 30s)
```

* Add a new entry (specific group)

```
$ passtis add -u user -U https://wiki.workplace.com -g work wiki.workplace.com
Password: 
Confirm password:
```

* List entries

```
$ passtis list
[Password Store]
├── default
│   ├── example.com
│   ├── github.com
│   └── google.com
├── social
│   ├── facebook.com
│   └── twitter.com
└── work
    ├── gitlab.workplace.com
    ├── wiki.workplace.com
    └── workplace.com
```

* List entries (only specific groups)

```
$ passtis list -G work social
[Password Store]
├── social
│   ├── facebook.com
│   └── twitter.com
└── work
    ├── gitlab.workplace.com
    ├── wiki.workplace.com
    └── workplace.com
```

* Get an entry's content

```
$ passtis get example.com
---------- default/example.com ----------
URI      : http//example.com
Username : user
Comment  : very password, much secure
-----------------------------------------
password copied to clipboard (will be cleared in 30s)
```

* Get an entry's content (specific group)

```
$ passtis get -g work wiki.workplace.com
---------- work/wiki.workplace.com ----------
URI      : http//wiki.workplace.com
Username : user
Comment  : 
---------------------------------------------
password copied to clipboard (will be cleared in 30s)
```

* Get an entry's content (display password)

```
$ passtis get -e example.com
---------- default/example.com ----------
URI      : http//example.com
Username : user
Comment  : very password, much secure
Password : password
-----------------------------------------
```

## Full help

```
usage: passtis [-h] [-v] [-d DIR] [-V] {init,add,del,list,get} ...

Passtis - Command line password manager.

positional arguments:
  {init,add,del,list,get}
    init                initialize the password store
    add                 add a new entry
    del                 delete an entry
    list                list store entries
    get                 fetch an entry from the store

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit
  -d DIR, --dir DIR     store location
  -V, --verbose         display GnuPG debug information
```

## init

```
usage: passtis init [-h] key_id

positional arguments:
  key_id      ID of the key used for encrypting the store

optional arguments:
  -h, --help  show this help message and exit
```

## add

```
usage: passtis add [-h] [-u USER] [-U URI] [-c COMMENT] [-g GROUP]
                   [--generate]
                   name

positional arguments:
  name                  entry name

optional arguments:
  -h, --help            show this help message and exit
  -u USER, --user USER  user name
  -U URI, --uri URI     resource URI
  -c COMMENT, --comment COMMENT
                        additional entry information
  -g GROUP, --group GROUP
                        group the entry belongs to
  --generate            generate random password
```

## del

```
usage: passtis del [-h] [-g GROUP] [-y] name

positional arguments:
  name                  entry name

optional arguments:
  -h, --help            show this help message and exit
  -g GROUP, --group GROUP
                        group the entry belongs to
  -y, --yes             do not ask for confirmation
```

## list

```
usage: passtis list [-h] [-G GROUP [GROUP ...]]

optional arguments:
  -h, --help            show this help message and exit
  -G GROUP [GROUP ...], --groups GROUP [GROUP ...]
                        only display entries from given groups
```

## get

```
usage: passtis get [-h] [-g GROUP] [-e] [-s] name

positional arguments:
  name                  entry name

optional arguments:
  -h, --help            show this help message and exit
  -g GROUP, --group GROUP
                        group the entry belongs to
  -e, --echo            display password instead of copying it to the
                        clipboard
  -s, --silent          do not output anything
```

## Licensing

Passtis is licensed under the GNU General Public License v3 (GPLv3).
