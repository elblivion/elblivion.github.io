+++
date = "2016-05-29T01:22:14+02:00"
title = "Slimmer Ubuntu PPA in docker"

+++

Many of my Docker images for testing software for production are based on Ubuntu, because that's  what we use in production and while I love Alpine Linux-based images it's  just too much overhead, not to mention self-defeating, to be hunting for equivalent packages all the time in a different distro and dealing with dynamic linking issues. Still, that image size tho!

<!--more-->

```
ubuntu   14.04               b549a9959a66        7 weeks ago         188 MB
alpine   3.3                 d7a513a663c1        8 weeks ago         4.798 MB
```

One annoying thing, though, is when I need a package from a PPA. Since it makes it self-contained in the Dockerfile we began using the `add-apt-repository`, but this includes Python and lots of dependencies, which is OK on a VM but makes the container extremely heavy.

Recently while building [Openresty](http://openresty.org/) in a container I simply wanted to add [Bats](https://github.com/sstephenson/bats) to a testing container, and pulling this whole Python environment slowed the testing step down - so I used the following, which requires an extra file but is way more lightweight. You also need to manually specify the PGP key ID of the PPA, but I'm not sure having to check that is a bad thing. :-)

```
# my base container, based on ubuntu:14.04
FROM openresty-trusty

# Install bats
ADD files/bats.list /etc/apt/sources.list.d
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5F28EA3F && \
  apt-get update && \
  apt-get install -y bats && \
  rm -rf /var/lib/apt/lists/*

COPY . /app

WORKDIR /app

CMD ["./scripts/test.sh"]
```

```
$ cat files/bats.list
deb http://ppa.launchpad.net/duggan/bats/ubuntu trusty main
deb-src http://ppa.launchpad.net/duggan/bats/ubuntu trusty main
```

The difference in image size is "only" 40 MB, but the build time is 6x faster (30s vs 3 mins).
