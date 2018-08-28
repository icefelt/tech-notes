---
categories:
  - Other
tags: 
  - chef
  - chef-client
  - knife 
  - ssh 
title: "Use Chef's knife ssh command to run chef-client on multiple instances"
date: "2017-12-15T16:49:08-08:00"
draft: false
---
Example using Chef's knife command to run chef-client on all instances using the dev_identity.pem. 
```
knife ssh "role:*" "chef-client" -i ~/.ec2/dev_identity.pem
```

