[Andrew M. Riley](/)

-   [Blog](/)
-   [Projects](/about/#projects)
-   [Presentations](/about/#presentations)
-   [About Me](/about/)

My Daily Vagrant Tricks {.post-title}
=======================

2014/03/19 (3 minute read)

#### Table of Contents

-   [vbguest](#vbguest:2f08f961883eea8fbccf993e760915d4)
-   [hostmanager](#hostmanager:2f08f961883eea8fbccf993e760915d4)
-   [NFS](#nfs:2f08f961883eea8fbccf993e760915d4)

I use Vagrant daily to take care of my various dev environments. Its the
only reliable way I can juggle different Apache, MySQL, PHP,
SASS/Compass versions, etc. I run one vagrant environment per site so I
end up having quite a few vagrants halted on my system. I use the
following tools to help make my management of my environments that much
easier.

vbguest {#vbguest:2f08f961883eea8fbccf993e760915d4}
=======

[This plugin allows you to automatically install the host’s VirtualBox
Guest Additions (or auto-upgrade
them.)](https://github.com/dotless-de/vagrant-vbguest) I use this
because when I create a new vagrant install I really hate having to go
and look up the commands to install/compile the VirtualBox Guest
Additions. I also used to stick with old versions of VirtualBox just so
I wouldn’t have to take the time to upgrade my Guest Additions. Now I
can just upgrade VirtualBox as needed and this plugin takes care of the
additions without any work from me.

To install just run:

    vagrant plugin install vagrant-vbguest

No config modifications or commands are required.

hostmanager {#hostmanager:2f08f961883eea8fbccf993e760915d4}
===========

I got tired of having to manually edit my /etc/hosts to make urls like
andrew.dev resolve. [This plugin takes care of setting both your host
and guest hosts file entries for you (depending on
config.)](https://github.com/smdahlen/vagrant-hostmanager) This is great
if you ever run multiple Vagrant boxes that need to talk to each other.

To install just run:

    vagrant plugin install vagrant-hostmanager

If you want to have this plugin update your host systems /etc/hosts file
you’ll need to add the following to your Vagrantfile:

    config.vm.hostname = 'yourhostname.blah'
    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true

Be sure to check the readme file in the [git
repo](https://github.com/smdahlen/vagrant-hostmanager) for all the
options.

If you use .dev for your TLD Chrome will try to search instead of going
to the site. Enter a “/” after it to tell Chrome that it is a url.

NFS {#nfs:2f08f961883eea8fbccf993e760915d4}
===

I edit my files using [PHPStorm](http://www.jetbrains.com/phpstorm/)
(with the Vim plugin) in my host system so I need a way to get my files
from OS X to my Linux guest. By default Vagrant/VirtualBox provides
multiple ways to get the files from one system to another. I use the NFS
option to avoid the default memory mapped option since it is very slow
when you have thousands of little source files (as Drupal tends to).

Just add the

    , type: "nfs"

to the end of your standard

    config.vm.synced_folder "repopathgoeshere", "/vagrant"

line to make it

    config.vm.synced_folder "repopathgoeshere", "/vagrant", type: "nfs"

One thing to note, if you are running Compass/SASS on your guest system,
there is a noticeable lag from when you save the file on your host
system to when it recompiles on your guest system (I’ve seen the delay
be as bad as a minute.) If you are trying to do something like this I’d
recommend you try the new [rsync option available in Vagrant 1.5 and
later](https://www.vagrantup.com/blog/feature-preview-vagrant-1-5-rsync.html).

Categories: [development](/categories/development)

Tags: [vagrant](/tags/vagrant) [LAMP](/tags/lamp) [Linux](/tags/linux)

This work by [Andrew M.
Riley](https://plus.google.com/114623748656325106111) is licensed under
a [Creative Commons Attribution 3.0 Unported
License](http://creativecommons.org/licenses/by/3.0/)
