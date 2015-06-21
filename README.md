# Server Setup

* set up mx record for new domain to have mx as my existing domain
* allow new domain in postfix, add an alias for postmaster@
* log into server, do aptitude update.  Still 14.04.1 ubuntu
* fix missing files on my cloud hosting server.  Thanks cloud hosting provider!  They delete all `/etc/cron.daily` files on their ubuntu image.
  * `sudo aptitude install debsums`
  * `sudo debsums -a | grep -v OK`
  * `for F in $pkgs; do sudo apt-get install -o Dpkg::Options::="--force-confmiss" --reinstall $F; done`
* add andy@ user, with sudo: `usermod -G sudo -a andy`
* add minecraft@ user, regular.  `passwd -d minecraft`
* change root password. `sudo passwd`
* get rid of 'user' user:  `sudo deluser --remove-home user`
* add ssh key to andy@ user, `.ssh/authorized_keys`
* `sudo aptitude install vim-nox`
  * `sudo update-alternatives --config editor`
* `sudo vim /etc/hostname; sudo hostname $( cat /etc/hostname )`
* `sudo aptitude install bikeshed` - brings in a bunch of things, but I want purge-old-kernels script
* `sudo aptitude install unattended-upgrades`; https://help.ubuntu.com/lts/serverguide/automatic-updates.html
  * modify 50unattended-upgrades and 10periodic in /etc/apt/apt.conf.d
* `sudo aptitude install heirloom-mailx` ; needed by unattended-upgrades emailing
* `sudo aptitude install postfix` ; to send mail
  * Internet Site
  * edit /etc/aliases and add root: root@ entry
  * `sudo newaliases`
  * edit /etc/postfix/main.cf
    * `inet_interfaces = loopback-only`
* `sudo aptitude purge command-not-found` - I really don't like ubuntu being helpful
* `sudo aptitude install molly-guard`
* `sudo vim /etc/ssh/sshd_config`
```
# move the ssh port, to keep out the chumps
Port 22001

# don't keep the prompt open for so long
LoginGraceTime 30

# no root login
PermitRootLogin no
```
* fail2ban for the chumps that find the open port
  * https://github.com/andyjack/fail2ban

* set your watches `sudo aptitude install openntpd`

# MineCraft Setup

* java - is oracle best these days?
  * http://www.webupd8.org/2012/09/install-oracle-java-8-in-ubuntu-via-ppa.html
  * `sudo add-apt-repository ppa:webupd8team/java`
  * `sudo aptitude update`
  * `sudo aptitude install oracle-java8-installer`
  * Allow automatic license accept
  * `echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections`
* spigot mc
  * http://www.spigotmc.org/wiki/spigot-installation/#linux
  * http://www.spigotmc.org/wiki/buildtools/#linux
* as minecraft user:
```
cd ~
mkdir build
cd build
curl -O -L 'https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar'
java -jar BuildTools.jar
```
