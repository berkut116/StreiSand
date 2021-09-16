# Streisand 2021 (UPD for Ubuntu 20.04)

This is my fork for <i>kassemz/streisand</i> (forked from <i>StreisandEffect/streisand</i>)

Checked and proved to work on Oracle Cloud Ubuntu 20.04 arm64

<br>

<b> Use this settings while installation: </b>

<code> 7. localhost (Advanced) </code>

 you may leave choise in state <code>[yes]</code> in all other options exept last one (for me it was broken):
 
<code> [BROKEN ON SOME PROVIDERS, including AWS] Enable DNS-over-HTTPS (cloudflared)? Press enter for default  [no]: </code>

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

<b>Step 1:</b> Prepare Server and install all dependencies
In this step, we shall get everything we need for the entire installation process installed. Run the commands below to get everything ready on the builder server (your local machine)

-----Ubuntu-----

<code> sudo apt-get update </code>

<code> sudo apt-get install git python3 python3-venv python3-pip python3-dev python3-setuptools python-cffi  libcurl4-openssl-dev libffi-dev -y </code>
   
<b>Step 2</b>: Generate SSH Public Keys
We need authentication via keys between our local machine and the server sitting in the cloud. You can easily generate ssh keys as follows. Enter another file in which to save the key if you do not like the default. You can leave the passphrase empty.

<code> ssh-keygen </code>

Copy the public key to the remote server
In order for authentication via keys to happen, we have to copy the public key we just generated above to the remote server where Streisand will be installed. 

<b>Step 3:</b> Clone Streisand’s repository and prepare for installation
On your local machine, clone Streisand’s repository and prepare to install the server

<code> cd ~ </code>

<code> git clone https://github.com/StreisandEffect/streisand.git </code>

<code> cd streisand </code>

Run the installer for Ansible and its dependencies. The installer will detect missing packages, and print the commands needed to install them. (Ignore the Python 2.7 DEPRECATION warning; ignore the warning from python-novaclient that pbr 5.1.3 is incompatible.) If all packages it needs are present, it will proceed to install necessary tools it needs so that installation of Streisand goes smoothly

<code> ./util/venv-dependencies.sh ./venv </code>

In case you find any missing packages in the output, kindly install them depending on the environment your local machine is on.

<b>Step 4:</b> Install Streisand
While still in the same Streisand cloned directory, activate the Ansible packages that were installed in the previous step as follows

<code> source ./venv/bin/activate </code>

Then execute the Streisand script. You should see an output as shown below the command. Choose where your server sits. For this example, I will go with “Existing Server“

<code> ./streisand </code>

Once Ansible Play begins, pay key attention to the questions and options you will be required to input as the installation goes on. 

<b>Step 5:</b> Configure your clients
Once Streisand completes installation, you will find complete instructions to setup clients in <code>“~/streisand/generated-docs”</code> directory. Especially in <code>“vagrant-dev.html”</code> file. Below the file, you will also find how to login to your streisand instance where you will get the same documentation.

Login by pointing your browser to <code>https://IP-or-FQDN</code> of your server. You will get a login prompt. Enter the username and password found at the bottom the file
And you will be ushered into the documentation page. Therein you will find various ways that you can connect to your Streisand Gateway server using various clients.

<b>Concluding Remarks</b>

Once you connect to your Streisand Gateway server, your IP is protected and you can access restricted content found in other countries. Get naughty this festive season by installing Streisand which incoporates Ansible in its installation and see the results that you will get. As you celebrate with the ones you care about, we continue to appreciate your relentless support and we wish you a marvelous time. Do not forget to keep safe.

## iptables rules (for case when ufw not working - bug on Oracle VPS)

<code>iptables -I POSTROUTING -o enp0s3 -j MASQUERADE</code>
 
<code>iptables -I INPUT -p tcp -m tcp --dport 22 -j ACCEPT -m comment --comment "SSH"</code>

<code>iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT -m comment --comment "nginx"</code>
 
<code>iptables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT -m comment --comment "sslh"</code>

<code>iptables -I INPUT -p tcp -m tcp --dport 636 -j ACCEPT -m comment --comment "openvpn"</code>
 
<code>iptables -I INPUT -p tcp -m tcp --dport 993 -j ACCEPT -m comment --comment "stunnel"</code>
 
<code>iptables -I INPUT -p tcp -m tcp --dport 8443 -j ACCEPT -m comment --comment "tor"</code>
 
<code>iptables -I INPUT -p tcp -m tcp --dport 9443 -j ACCEPT -m comment --comment "obfs4proxy"</code>
 
<code>iptables -I INPUT -p tcp -m tcp --dport 4443 -j ACCEPT -m comment --comment "ocserv"</code>
 
<code>iptables -I INPUT -p tcp -m tcp --dport 8530 -j ACCEPT -m comment --comment "v2ray plugin"</code>

<code>iptables -I INPUT -p udp -m udp --dport 4443 -j ACCEPT -m comment --comment "ocserv" </code>
 
<code>iptables -I INPUT -p udp -m udp --dport 8530 -j ACCEPT -m comment --comment "shadowsocks"</code>
 
<code>iptables -I INPUT -p udp -m udp --dport 8757 -j ACCEPT -m comment --comment "openvpn"</code>
 
<code>iptables -I INPUT -p udp -m udp --dport 51820 -j ACCEPT -m comment --comment "Wireguard"</code>

<code>iptables -I INPUT -s 192.168.1.0/24 -p udp -m udp --dport 53 -j ACCEPT -m comment --comment "DNS"</code>
 
<code>iptables -I INPUT -s 10.8.0.0/24 -p udp -m udp --dport 53 -j ACCEPT -m comment --comment "DNS"</code>
 
<code>iptables -I INPUT -s 10.9.0.0/24 -p udp -m udp --dport 53 -j ACCEPT -m comment --comment "DNS"</code>
 
<code>iptables -I INPUT -s 10.192.122.0/24 -p udp -m udp --dport 53 -j ACCEPT -m comment --comment "DNS"</code>

<code>iptables -I INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT</code>

<code>iptables -I FORWARD -j ACCEPT</code>
 
<code>iptables -I OUTPUT -j ACCEPT</code>

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
