---
title: Installation
layout: guide
tags: ['guide', page']
guideOrder: 10
---
## Raspian Jessie Installation
Install Raspian Jessie on your SD-Card from [raspberrypi.org](https://www.raspberrypi.org/downloads/).
Start your Raspberry Pi and connect with SSH or Putty.

Raspian Jessie insalled an old node.js version, you have to deinstall it:

sudo apt-get purge nodejs

If you want a static IP open dhcpcd.conf:

sudo nano /etc/dhcpcd.conf

and add this at the end (edit the IP-Adress to your Network):

interface eth0
static ip_address=192.168.178.190/24
static routers=192.168.178.1
static domain_name_servers=192.168.178.1 8.8.8.8 4.2.2.1

Now you can configure Raspian with raspi-config:

sudo raspi-config
1. Expand Filesystem
2. Change User Password
3. Internationalisation Options
3.1 Change Locale for German users "de_DE.UTF-8 UTF-8" aktivate with the space key and disable "en_GB.UTF8 UTF8" with the space kek then "ok"
3.1.1 Now aktivate "de_DE.UTF-8" and "ok"
go a second time to Internationalisation Options
3.2 Change Timezone and aktivate your timezone
then we are ready and change "Finish"

I recomered to reboot now.

sudo reboot

Now we update Raspian:

sudo apt-get update && sudo apt-get upgrade

It’s a good time for a reboot

sudo reboot

## Node.js Installation

First you need to install [node.js](http://nodejs.org) that comes with the package manager
[npm](https://npmjs.org/).

You need to install or update to the __LTS__ version of Node.js ([version 4.4.5](https://nodejs.org/en/download/) at 
the time of writing). Earlier versions of Node.js aren't supported. 

If you are on the Raspberry Pi and running the standard Raspbian distribution you can use the following installation 
procedure. 

__Pi Model A, B, B+ or Zero__

    wget https://nodejs.org/dist/v4.4.5/node-v4.4.5-linux-armv6l.tar.gz -P /tmp
    cd /usr/local
    sudo tar xzvf /tmp/node-v4.4.5-linux-armv6l.tar.gz --strip=1
    
__Pi 2 Mode B or Pi 3 Model B__

    wget https://nodejs.org/dist/v4.4.5/node-v4.4.5-linux-armv7l.tar.gz -P /tmp
    cd /usr/local
    sudo tar xzvf /tmp/node-v4.4.5-linux-armv7l.tar.gz --strip=1
        
To install node on another platform than Raspberry Pi have a look at 
[installing Node.js via package manager](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions).

Check you Node.js version with:

    /usr/bin/env node --version

## pimatic Installation

You must have the `gcc` compiler or some other suitable compiler installed. Moreover, you need to have `git` installed. 
On Debian-based systems run:

    sudo apt-get install build-essential git

Once node.js and npm are installed you can run

    cd /home/pi
    mkdir pimatic-app
    npm install pimatic --prefix pimatic-app --production

to install the pimatic framework.

Copy the default config file:

    cd pimatic-app
    cp ./node_modules/pimatic/config_default.json ./config.json

You should end up with these files in your `pimatic-app` directory:

<table class="table file-listing">
<tr><td>`config.json`</td>				       <td>the config file</td></tr>
<tr><td>`node_modules`</td>				       <td>directory for the framework and plugins</td></tr>
<tr><td>`node_modules/pimatic`</td>			   <td>the pimatic framework files</td></tr>
</table>

Now, you need to set the password for the admin user. Open the file `config.json` using a text editor (e.g., `nano`)
and search for the string `"users"`. Then, change the value of the `password` property for user "admin" below.

Now we can start pimatic for the first time:

sudo node /home/pi/pimatic-app/node_modules/pimatic/pimatic.js

You see the debug messages from Pimatic if the end shows:

19:31:55.621 [pimatic-mobile-frontend] packing static assets
19:31:56.324 [pimatic-mobile-frontend] packing static assets finished
19:31:56.339 [pimatic-mobile-frontend] rendering html
19:32:10.747 [pimatic-mobile-frontend] rendering html finished
19:32:10.835 [pimatic] Listening for HTTP-request on port 80...

You can try to connect with your web browser.
If all is OK you stop Pimatic by pressing “Strg+C” on the keyboard.
