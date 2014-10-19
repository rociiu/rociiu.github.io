---
layout: post
title: "Issues of upgrading to Yosemite related to homebrew"
date: 2014-10-19 14:59:58 +0800
comments: true
categories: 
---

## Fix brew command

In Yosemite the ruby command path changed, yosemite use ruby 2.0 as default also create a new path to link to ruby 2.0:

    /System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/ruby

You need to change ruby path in the first line of /usr/local/Library/brew.rb 


## Update brew

After fix the brew command, it's better to update brew itself to avoid other issues

    brew update

## Fix postgresql

After upgrade, you'll get following error when try to run: postgres -D /usr/local/var/postgresql

    FATAL:  could not open directory "pg_tblspc": No such file or directory

the fix is easy, just create those directories, those directories are gone after upgrade.

    mkdir /usr/local/var/postgres/pg_tblspc
    mkdir /usr/local/var/postgres/pg_twophase
    mkdir /usr/local/var/postgres/pg_stat_tmp


## install xcode, run 'xcode select --install' to install command tools



## references

  [http://stackoverflow.com/questions/25970132/pg-tblspc-missing-after-installation-of-os-x-yosemite-beta/26001639#26001639](http://stackoverflow.com/questions/25970132/pg-tblspc-missing-after-installation-of-os-x-yosemite-beta/26001639#26001639)
  [https://jimlindley.com/blog/yosemite-upgrade-homebrew-tips/](https://jimlindley.com/blog/yosemite-upgrade-homebrew-tips/)

