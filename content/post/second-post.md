---
categories:
  - Others
tags: 
  - linux
  - add users
  - centos
title: "In Centos, add user"
slug: "Adding users in centos"
date: "2017-09-25T16:13:57-07:00"
draft: false
---

Manusal steps to add a user with sudo accecss in centos
```
adduser -U -G adm,wheel username -s /bin/bash && su -l username && mkdir .ssh && chmod 0700 .ssh && cd .ssh/

vi authorized_keys

ssh-rsa AAAAB3NzaThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFakeThisIsFake underthehood@grossinhere.local

chmod 0600 authorized_keys
```


