---
layout: post
title: Installing Discourse on Ubuntu Trusty 32-bit
---

Last time I evaluated [comment systems][] suitable for a static blog
such as this and came with a conclusion that [Discourse][] is the one
and only solution (short of rolling my own).

[comment systems]: comment-system
[Discourse]: http://www.discourse.org/

However, this requires setting up a Discourse instance.

I naturally want to install it on my home server that does my
multiprovider Internet routing, powers the whole apartment networking
infrastructure (DHCP, local DNS, those sorts of things), hosts a few
playground projects, and is overall a not-so-shabby machine that I’m
not very keen on upgrading.

----

## What’s the problem, anyway?

The quirk is that this so-called server is what used to be my previous
desktop. Namely, a Pentium 4, family 15, model 2, stepping 9, which
means it’s a Northwood, the last Pentium 4 family that *did not*
support 64-bit instructions.

Now, [Discourse is primarily distributed as a Docker container][1]. And
Docker only runs on 64-bit machines.

[1]: https://github.com/discourse/discourse/blob/master/docs/INSTALL.md

So the Docker installation scenario is out for me. No problem, I hear
there also is a [manual installation guide][].

[manual installation guide]: https://github.com/discourse/discourse/blob/master/docs/INSTALL-ubuntu.md


## Hardware requirements

The guide recommends 2G RAM, 2G swap, and two processor cores. It
lists the minimum as 1G RAM, 3G swap and a single core. I have 1G RAM,
3G swap and a single hyperthreaded core, and I don’t estimate a very
large load, so expect it to cope.


## Dependencies

So I take my fully up-to-date Ubuntu 14.04 server. OpenSSH: check.
Mail server: check. PostgreSQL: check. Git: check. Redis: `aptitude
install redis-server`, check. Nginx: check. Ruby 2.0? `aptitude
install ruby2.0`, check.

Now what’s this, RVM? Ruby Version Manager? Downloads and builds any
version of Ruby right in your unprivileged user’s home directory? Oh
no you don’t! I have a properly deb-packaged system-wide installations
of Ruby 1.9.1 and Ruby 2.0.0, with a selection of properly
deb-packaged Ruby Gems, and I intend to use those as much as possible.

So let’s skip that. Instead, I create a `discourse` user account, add
it to the `sudo` group (for the duration of installation only), create
a `/home/discourse/bin` directory and symlink the binaries from the
`ruby2.0` package into there, under their unversioned names:

```bash
dpkg -L ruby2.0|sed -nE 's:^(/usr/bin/(.*)2\.0)$:ln -s \1 /home/discourse/\2:p'
```

This way, logging in as `discourse` and typing `ruby --version`, I get:

```
ruby 2.0.0p384 (2014-01-12) [i386-linux-gnu]
```


## Gems

Now, the guide says `gem install bundler`. Since bundler is packaged
by Ubuntu, I want to use that instead. `sudo aptitude install
bundler`.

Next, fork and clone the repository and check out a stable release.

```bash
install -m 755 -o discourse -g discourse -d /var/www-discourse
cd /var/www-discourse
git clone https://github.com/yurikhan/discourse.git .
git checkout latest-release
```

The next step seems to be installing necessary gems. This is a bit of
a problem because the default suggested way is by using Bundler, which
will attempt to download and install any necessary gems that I don’t
already have installed. This will use the rubygems.org index rather
than the Ubuntu package repository, and that’s a concern for me. I’d
rather prefer if applications used system-provided libraries, not
bring everything they need with them.

At the same time, I don’t want to resolve all the dependencies by
myself. So let it do its thing, for now.

```bash
sudo aptitude install ruby2.0-dev postgresql-server-dev-9.3 libpq-dev
bundle install --deployment --without development:test
```

It tells me my bundler is out-of-date and if I could please update it.
No I won’t. From what I understand it’s just a (secondary) package
manager, and if my primary package management system says it strikes a
good balance between stable and featureful, then so be it.

However, it goes on downloading and installing. In the process, it
tells me that a gem called `nokogiri` comes with its custom version of
`libxml2` and `libxslt` because libxml2 2.9.0 is known to be broken.
Well, duh! If it’s broken, declare a package dependency on `libxml2
(<< 2.9.0)` and be done with it. Oh shit, you are a ruby gem and
cannot declare deb package dependencies. Tough luck. Well, I’ll deal
with you later.

All in all, gem installation went without a hitch. (Not really; guess
how I knew to install those three `*-dev` packages above.) As far as I
can tell, gems were installed right into the project directory, under
`vendor/bundle`, whose final size is some 280M. In the `/var`
hierarchy, which is supposed to contain only files that are at least
sometimes modified. Totally inappropriate.


## Configuration

This step is pretty straightforward.


## Nginx configuration

The guide says copy `nginx.global.conf` and `nginx.sample.conf` to
`/etc/nginx/conf.d` as `local-server.conf` and `discourse.conf`,
respectively. On Ubuntu, virtual server configs are supposed to be in
`/etc/nginx/sites-available` with a symlink from
`/etc/nginx/sites-enabled`, so `nginx.sample.conf` goes there. And I
don’t initially see the need for tweaking
`server_names_hash_bucket_size` which is all that `local-server.conf`
does, so let’s try without that first.

On reload attempt, nginx complains about the cache directory which is
set in `discourse` as `/var/cache/nginx`. Let’s create that.

While we are at it, let’s also configure logging to a separate
directory.

```
access_log /var/log/nginx/discourse/access.log;
error_log /var/log/nginx/discourse/error.log;
```

```bash
sudo install -m 755 -o www-data -g adm -D /var/cache/nginx /var/log/nginx/discourse
```


## Managing services

The guide suggests installing a gem called Bluepill and running
Discourse using the `config/discourse.pill` file.

Bluepill is in fact a service monitoring daemon written in Ruby. This
means we can do without it, as Ubuntu already has two service
managers: SysV init and Upstart.

As far as I can tell, we need two services running: `sidekiq` and
`thin`. Let’s start with the latter as it is simpler.

Actually, I already use `thin` for a Redmine instance, so I am a
little familiar with its configuration. Basically, you put your `thin`
configs into `/etc/thin1.9.1` or `/etc/thin2.0`, and the
Ubuntu-packaged `/etc/init.d/thin` script takes care of the rest.

The config goes roughly like this:

    ---
    pid: /var/www-discourse/tmp/pids/thin.pid
    wait: 30
    timeout: 30
    log: /var/www-discourse/log/thin.log
    max_conns: 1024
    require: []
    
    environment: production
    max_persistent_conns: 512
    no-epoll: true
    servers: 4
    daemonize: true
    socket: /var/www-discourse/tmp/sockets/thin.sock
    chdir: /var/www-discourse
    user: discourse
    group: discourse

For `sidekiq`, there are Upstart configs in the
[upstream repository][sidekiq]. I adapt one for my Discourse instance:

[sidekiq]: https://github.com/mperham/sidekiq/blob/master/examples/upstart/manage-one/sidekiq.conf

    # /etc/init/discourse-sidekiq.conf - Sidekiq config
    #
    # Adapted from https://github.com/mperham/sidekiq/blob/master/examples/upstart/manage-one/sidekiq.conf
    
    description "Sidekiq Background Worker"
    
    start on runlevel [2345]
    stop on runlevel [06]
    
    # change to match your deployment user
    setuid discourse
    setgid discourse
    
    respawn
    respawn limit 3 30
    
    # TERM is sent by sidekiqctl when stopping sidekiq. Without declaring these as normal exit codes, it just respawns.
    normal exit 0 TERM
    
    script
    exec /bin/bash <<EOT
      export HOME=/home/discourse
      export PATH=/home/discourse/bin:/usr/local/bin:/usr/bin:/bin
      export PIDFILE=/var/www-discourse/tmp/pids/sidekiq-worker.pid
      export RAILS_ENV=production
      cd /var/www-discourse
      bundle exec sidekiq -L /var/www-discourse/log/sidekiq.log
    EOT
    end script

Now, after `restart thin && start sidekiq`, we should be up and running!

Except we’re not. `thin` logs say some gems cannot be found, and the
stack trace mentions `ruby1.9.1`.

I assume this is because the init script tries running
`/usr/bin/ruby2.0 /usr/bin/thin start --all /etc/thin2.0`, but
`/usr/bin/thin` has in its first line `#!/usr/bin/env ruby`. Where
`ruby` still refers to `ruby1.9.1` because we’re running as root at
this point.

So let’s copy `/usr/bin/thin` to `/usr/bin/thin2.0` and replace `ruby`
with `ruby2.0` in its shebang line, and also copy `/etc/init.d/thin`
to `/etc/init.d/thin2.0` and modify both init scripts to only run
their corresponding `thin` binaries.

This fixes `thin` startup. Now the assets (js and css) are not served,
returning 403 Forbidden instead. That’s because nginx is running as
`www-data` and the directory is owned by `discourse` and some of its
files and subdirectories got 660 (resp. 770) permissions when I cloned
the repository. Moreover, when the application creates new files, they
also get that mask. So it seems best to add `www-data` to the
`discourse` group.

After that, it actually works. Lets me register an account, sends an
activation email, and lets me enable Google and GitHub logins.
