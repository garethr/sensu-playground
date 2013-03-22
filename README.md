[Sensu](http://sensuapp.com) is a relatively new monitoring system based around a messaging
architecture. It's a simple idea, a small code base and provides
lots of ways of extensing the core featureset. What's not to like?

This repo contains a Sensu demo environment. Using
[Vagrant](http://vagrantup.com) 1.1+ and the
[sensu-puppet](https://github.com/sensu/sensu-puppet) module it brings up two nodes, one of which runs the
sensu server and dashboard, and the other a sensu client. On it's own
it's not very exciting, but it's a great way to have something running
in minutes with which to experiment. Improvements welcome.

You'll find lots of sample checks, metrics and handlers in the
[sensu-community-plugins](https://github.com/sensu/sensu-community-plugins) repo.

## Usage

First install vagrant, Bundler (Ruby dependeny management) and the
[librarian-puppet](https://github.com/rodjek/librarian-puppet) gem:

    gem install bundler
    bundle install

Librarian-puppet allows for managing puppet modules from the
[Forge](http://forge.puppetlabs.com) or from Git repositories. To
install the required third-party puppet modules run:

    bundle exec librarian-puppet install

Finally to bring up the two node sensu cluster run:

    vagrant up

The first time you run this it will take a while as it downloads a large
Vagrant basebox image. See the Vagrant documentation for more
information about managing the Vagrant created virtual machines.

All being well, everything should run cleaning and if you visit localhost:8080 you
should see the sensu dashboard. The username for the basic
authentication is admin and the password secret.

## A quick example

The check setup by the puppet manifest checks for the existence of a
file at /tmp/missingfile every 60 seconds. To start with these files
will be missing, so you should see a warning in the web interface. If
you jump on the virtual machine and create the file the warning should
eventually disappear.

    vagrant ssh server
    touch /tmp/missingfile

## Thanks

This repository mainly packages other peoples work:

* Thanks to @portertech and others for Sensu
* Thanks to @jamtur01, @jlambert121 and Jeremy Caroll for the excellent sensu-puppet module
