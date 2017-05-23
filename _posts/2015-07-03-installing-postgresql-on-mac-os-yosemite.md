---
title: Installing PostgreSQL on Mac OS Yosemite
date: 2015-07-03 06:41:42
categories: code
excerpt: Solving a common problem when installing PostgreSQL on Mac.
---

I faced some headache installing PostgreSQL on my Mac via [Homebrew](http://brew.sh/){:target="_blank"} (a package manager for MacOS kinda like apt-get for linux). After installed, I tried to use its console and:

    PostgreSQL error: Fatal: role “username” does not exist

After wander around the web and some StackOverflow questions later, I solved the problem by running in the console:

    createdb

The problem was that Postgres was looking for the default database which is named after your user name. **`createdb`**  without any arguments, does the trick by creating a database with your user name.
