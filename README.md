<h3 align="center">xudo</h3>

<div align="center">

[![Status](https://img.shields.io/badge/status-active-success.svg)]()
[![GitHub Issues](https://img.shields.io/github/issues/kylelobo/The-Documentation-Compendium.svg)](https://github.com/kylelobo/The-Documentation-Compendium/issues)
[![GitHub Pull Requests](https://img.shields.io/github/issues-pr/kylelobo/The-Documentation-Compendium.svg)](https://github.com/kylelobo/The-Documentation-Compendium/pulls)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](/LICENSE)

</div>

---

<p align="center"><b>TL;DR: sudo, but X and password- (and passphrase-) less ssh still work</b>
</p>

## 📝 Table of Contents

- [About](#about)
- [Getting Started](#getting_started)
  - [Prerequisites](#prerequisites)
  - [Installing](#installing)
  - [Testing the install](#test_install)
- [Usage](#usage)
  - [Example](#example)
- [Deployment](#deployment)
- [Built Using](#built_using)
- [TODO](../TODO.md)
- [Contributing](../CONTRIBUTING.md)
- [Authors](#authors)
- [Acknowledgments](#acknowledgement)

## About <a name = "about"></a>

On Unix when you escalate privileges (su root) or change to any other user you lose additional privileges that were granted your user:

- Authentication to any X windowing system a.k.a. "MIT-magic-cookie"
- Sockets to any SSH Agent, either set up by yourself or your terminal emulator (i.e. PuTTY)

In the old days we would use ssh root@localhost with either the -A or {-X|-Y} options. While this still works (and is an excellent way of doing this) it requires you to have a public key in root's authorized_keys file.

And many IT security departments frown on that.

A common practise is to copy the MIT-magic-cookie by hand or set permissions on the SSH agent's socket by hand. But that get's boring pretty fast...

This script does exactly that: it copys over the MIT-magic-cookie and makes the socket pointed to by SSH_AUTH_SOCK available to your target user (the user you sudo to).


## 🏁 Getting Started <a name = "getting_started"></a>

### Prerequisites <a name="prerequisites"></a>

You need the following binaries on your system:

- sudo: (obviously) - most if not all systems have that these days
- xauth: only if you want to forward X authentication - usually in a package of the same name
- setfacl: only if you want to forward SSH agents, usually found in an ACL package 

### Installing <a name="installing"></a>

#### Check out the repo:
```
git clone https://github.com/jayjay73/xudo.git
```
#### Copy the script to a directory in your path:
```
cp xudo ~/bin
```
#### make it executable:
```
chmod +x ~/bin/xudo
```
#### sudo to root taking your keys and X auth with you:
```
xudo -ax -iu root
```
#### Test if it works: <a name="test_install"></a>
```
xclock &
ssh someuser@another.host
```
both should now work without any further interruption.


## 🎈 Usage <a name="usage"></a>

```
xudo [-ax] [sudo options|command]

xudo options:

  -a    SSH auth forwarding
  -x    X auth forwarding
``` 

Invoking xudo has two parts: it's own options and the rest of the command line which is passed on to sudo.

For the options passed on to sudo see sudo's man page.

xudo only takes short options (with one dash "-"). Long options (with two dashes "--") are not supported.

### Example: <a name="example"></a>
```
xudo -a -iu ansible
```

Here -a is an option to xudo meaning forward SSH authentication, whereas -iu are options to sudo meaning perform a login (-i) as user (-u) ansible.

## 🚀 Deployment <a name = "deployment"></a>

Feel free to write a package or an Ansible playbook to get it deployed easily 🙂

## ⛏️ Built Using <a name = "built_using"></a>

- Bash

## ✍️ Authors <a name = "authors"></a>

- [@jayjay73](https://github.com/jayjay73)


## 🎉 Acknowledgements <a name = "acknowledgement"></a>

- All the DBAs that would patiently go through the motions of copying over their MIT-magic-cookies to install Oracle DB. I took pity on you.