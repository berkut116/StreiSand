# Streisand

<p align="center">
<img src="https://raw.githubusercontent.com/jlund/streisand/master/logo.jpg" alt="Automate the effect"/> 
</p>

- - -
[English](README.md), [Français](README-fr.md), [简体中文](README-chs.md), [Русский](README-ru.md) | [Mirror](https://gitlab.com/alimakki/streisand)
- - -

[![Build Status](https://github.com/StreisandEffect/streisand/workflows/Streisand/badge.svg)](https://github.com/StreisandEffect/streisand/actions)
[![Twitter](https://img.shields.io/twitter/url/https/twitter.com/espadrine.svg?style=social&label=Follow%20%40StreisandVPN)](https://twitter.com/StreisandVPN)

Streisand
=========

**Silence censorship. Automate the [effect](https://en.wikipedia.org/wiki/Streisand_effect).**

The Internet can be a little unfair. It's way too easy for ISPs, telecoms, politicians, and corporations to block access to the sites and information that you care about. But breaking through these restrictions is *tough*. Or is it?

If you have an account with a cloud computing provider, Streisand can set up a new node with many censorship-resistant VPN services nearly automatically. You'll need a little experience with a Unix command-line. (But without Streisand, it could take days for a skilled Unix administrator to configure these services securely!) At the end, you'll have a private website with software and instructions.

Here's what **[a sample Streisand server](http://streisandeffect-demo.s3-website.us-east-2.amazonaws.com/)** looks like.


There's a [list of supported cloud providers](#cloud-providers); experts may be able to use Streisand to install on many other cloud providers.

## VPN services

One type of tool that people use to avoid network censorship is a Virtual Private Network (VPN). There are many kinds of VPNs.

Not all network censorship is alike; in some places, it changes from day to day. Streisand provides many different VPN services to try. (You don't have to install them all, though.)

Some Streisand services include add-ons for further censorship and throttling resistance:

* [OpenSSH](https://www.openssh.com/)
    * [Tinyproxy](https://banu.com/tinyproxy/) may be used as an HTTP proxy.
* [OpenConnect](https://ocserv.gitlab.io/www/index.html) / [Cisco AnyConnect](https://www.cisco.com/c/en/us/products/security/anyconnect-secure-mobility-client/index.html)
    * This protocol is widely used by multi-national corporations, and might not be blocked.
* [OpenVPN](https://openvpn.net/index.php/open-source.html)
    * [Stunnel](https://www.stunnel.org/index.html) add-on available.
* [Shadowsocks](https://shadowsocks.org/en/index.html), 
    * The [V2ray-plugin](https://github.com/shadowsocks/v2ray-plugin) is installed to provide robust traffic evasion on hostile networks (especially those implementing quality of service (QOS) throttling).
* A private [Tor](https://www.torproject.org/) bridge relay
    * [Obfsproxy](https://www.torproject.org/projects/obfsproxy.html.en) with obfs4 available as an add-on.
* [WireGuard](https://www.wireguard.com/), a modern high-performance protocol.

See also:
* A [more technical list of features](Features.md) 
* A [more technical list of services](Services.md)

<a id="cloud-providers"></a>
## Cloud providers
* Amazon Web Services (AWS)
* Microsoft Azure
* Digital Ocean
* Google Compute Engine (GCE)
* Linode
* Rackspace


#### Other providers
We recommend using one of the above providers. If you are an expert and can set up a *fresh Ubuntu 16.04 server* elsewhere, there are "localhost" and "existing remote server" installation methods. For more information, see [the advanced installation instructions](Advanced%20installation.md).

## Installation

You need command-line access to a Unix system. You can use Linux, BSD, or macOS; on Windows 10, the Windows Subsystem for Linux (WSL) counts as Linux. 

Once you're ready, see the [full installation instructions](Installation.md).

Step 1: Prepare Server and install all dependencies
In this step, we shall get everything we need for the entire installation process installed. Run the commands below to get everything ready on the builder server (your local machine)

-----Ubuntu-----

$ sudo apt-get update
$ sudo apt-get install git python3 python3-venv python3-pip python3-dev python3-setuptools python-cffi  libcurl4-openssl-dev -y

----- Fedora -----
$ sudo dnf update
$ sudo dnf install git python3 gcc python3-devel python3-crypto \
     python3-pycurl libcurl-devel -y

-----CentOS 7-----
$ sudo yum -y update && sudo yum install -y epel-release
$ sudo yum -y update && sudo yum install -y \
    git gcc python36-devel python36-crypto python36-pycurl \
    libcurl-devel

-----macOS-----
$ brew install python3

Step 2: Generate SSH Public Keys
We need authentication via keys between our local machine and the server sitting in the cloud. You can easily generate ssh keys as follows. Enter another file in which to save the key if you do not like the default. You can leave the passphrase empty.

$ ssh-keygen

Copy the public key to the remote server
In order for authentication via keys to happen, we have to copy the public key we just generated above to the remote server where Streisand will be installed. 

Step 3: Clone Streisand’s repository and prepare for installation
On your local machine, clone Streisand’s repository and prepare to install the server

$ cd ~
$ git clone https://github.com/StreisandEffect/streisand.git
$ cd streisand
Run the installer for Ansible and its dependencies. The installer will detect missing packages, and print the commands needed to install them. (Ignore the Python 2.7 DEPRECATION warning; ignore the warning from python-novaclient that pbr 5.1.3 is incompatible.) If all packages it needs are present, it will proceed to install necessary tools it needs so that installation of Streisand goes smoothly

$ ./util/venv-dependencies.sh ./venv
Found a python3 command....
This system appears to be running Ubuntu or Debian. Checking
for critical packages.

Found: build-essential
Found: python3-pip
Found: python3-openssl
Found: python3-dev
Found: python3-setuptools
Found: python3-venv
Found: python-cffi
Found: libffi-dev
Found: libssl-dev
Found: libcurl4-openssl-dev
Found all critical packages.
In case you find any missing packages in the output, kindly install them depending on the environment your local machine is on.

Step 4: Install Streisand
While still in the same Streisand cloned directory, activate the Ansible packages that were installed in the previous step as follows

$ source ./venv/bin/activate
Then execute the Streisand script. You should see an output as shown below the command. Choose where your server sits. For this example, I will go with “Existing Server“

$ ./streisand

Once Ansible Play begins, pay key attention to the questions and options you will be required to input as the installation goes on. 

Step 5: Configure your clients
Once Streisand completes installation, you will find complete instructions to setup clients in “~/streisand/generated-docs” directory. Especially in “vagrant-dev.html” file. Below the file, you will also find how to login to your streisand instance where you will get the same documentation.

Login by pointing your browser to https://IP-or-FQDN of your server. You will get a login prompt. Enter the username and password found at the bottom the file
And you will be ushered into the documentation page. Therein you will find various ways that you can connect to your Streisand Gateway server using various clients.

Concluding Remarks
Once you connect to your Streisand Gateway server, your IP is protected and you can access restricted content found in other countries. Get naughty this festive season by installing Streisand which incoporates Ansible in its installation and see the results that you will get. As you celebrate with the ones you care about, we continue to appreciate your relentless support and we wish you a marvelous time. Do not forget to keep safe.

## Things we want to do better

Aside from a good deal of cleanup, we could really use:

* Easier setup.
* Faster adoption of new censorship-avoidance tools

We're looking for help with both.

If there is something that you think Streisand should do, or if you find a bug in its documentation or execution, please file a report on the [Issue Tracker](https://github.com/StreisandEffect/streisand/issues).

Core Contributors
----------------
* Jay Carlson (@nopdotcom)
* Nick Clarke (@nickolasclarke)
* Joshua Lund (@jlund)
* Ali Makki (@alimakki)
* Daniel McCarney (@cpu)
* Corban Raun (@CorbanR)

Acknowledgements
----------------
[Jason A. Donenfeld](https://www.zx2c4.com/) deserves a lot of credit for being brave enough to reimagine what a modern VPN should look like and for coming up with something as good as [WireGuard](https://www.wireguard.com/). He has our sincere thanks for all of his patient help and high-quality feedback.

We are grateful to [Trevor Smith](https://github.com/trevorsmith) for his massive contributions. He suggested the Gateway approach, provided tons of invaluable feedback, made *everything* look better, and developed the HTML template that served as the inspiration to take things to the next level before Streisand's public release.

Huge thanks to [Paul Wouters](https://nohats.ca/) of [The Libreswan Project](https://libreswan.org/) for his generous help troubleshooting the L2TP/IPsec setup.

[Starcadian's](https://www.starcadian.com/) 'Sunset Blood' album was played on repeat approximately 300 times during the first few months of work on the project in early 2014.
