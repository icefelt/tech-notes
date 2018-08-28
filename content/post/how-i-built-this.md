---
categories:
  - Others
tags:
  - hugo
  - intro
title: "First Post! Procedure to create and deploy this static site using Hugo, AWS S3, AWS Cloudfront, AWS Route53, and AWS Certificate Manager"
date: "2018-07-28T15:47:17-07:00"
draft: false
---

This first post are my notes to setup and configure this site. I leaned heavily on these sites to help me build this:
```
https://agop.me/post/https-enabled-portfolio-hugo-s3-cloudfront.html
https://fillmem.com/post/fast-secured-and-free-static-site/
```

This site must
- use my domain name, eissfeldt.com and my DNS severs (AWS Route53)
- use SSL certs (AWS Certificate Manager)
- be hosted in cheap cloud storage, not a server (AWS S3)
- use a CDN and and defaults to https (AWS CloudFront)

This post assumes 
- AWS account and AWS CLI are configured 
- git and npm installed in the developer's environment

## Procedure to create and deploy a static site using Hugo, AWS S3, AWS Cloudfront, AWS Route53, and AWS Certificate Manager

## Setup AWS account and configure AWS CLI
The AWS account, permissions, and the AWS CLI should already by setup. I'm not covering this in this post. 

To verify, list the AWS S3 buckets. This shows your AWS account is setup, you have access, you have permissions to see the buckets, and the AWS CLI is setup. 
```
$ aws s3 ls
```

## Create an AWS S3 bucket to host our static site named notes-blog-eissfeldt
```
$ aws s3 mb s3://notes-blog-eissfeldt
make_bucket: notes-blog-eissfeldt
```

To verify, use aws s3 ls command to see the bucket we created
```
$ aws s3 ls
2018-07-28 11:09:58 notes-blog-eissfeldt
```

## Create index.html placeholder until hugo site is pushed to S3 bucket. 
```
$ echo "Scott's Tech Notes here soon..." > index.html
$ aws s3 cp index.html s3://notes-blog-eissfeldt/index.html
```

To verify, navigate to the newly formed index.html in a browser
Note: browser will show access denied as this file is not public
```
https://s3.amazonaws.com/notes-blog-eissfeldt/index.html
```

## Create CloudFront distribution 
I leaned heavily on Step 4: Create a CloudFront distribution to act as your server
https://agop.me/post/https-enabled-portfolio-hugo-s3-cloudfront.html

I did this step through the console, not the AWS CLI. These notes could be better. 

I used DNS verification. There's a web button to add a DNS name to Route53 for me. It was the easy button. This may take 30 minutes. 

```
The DNS record was written to your Route 53 hosted zone. It may take up to 30 minutes for the changes to propagate, and for AWS to validate the domain.
```

I finished the CloudFront distribution, and I have my CNAME alias for notes.eissfeldt.com
```
ThisIsFake.cloudfront.net
```

I'm waiting for the CloudFront distribution to finish....

In AWS Route53, add a CNAME for notes.eissfeldt.com with the value from the cloudfront dns name from above

To verify this is done, you should be able to see the index page we created earlier in both the CloudFront DNS name as well as the 
``` 
ThisIsFake.cloudfront.net
notes.eissfeldt.com
```

!!!!!

## Setup git repo in this directory as we're going to pull the theme from a git submodule. More info here: 
https://minimo.netlify.com/docs/installation/

To add an existing project to GitHub using the command line:
https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/

```
$ git init 
$ git add . 
$ git commit -m "Initial Commit"
```

Successful Output is the newly created repo with an initial commit:
git@github.com:icefelt/notes-blog-eissfeldt.git


## Install hugo
```
$ brew install hugo
$ hugo version
Hugo Static Site Generator v0.45.1/extended darwin/amd64 BuildDate: unknown
```

## Create new hugo site named notes-blog-eissfeldt
Create new hugo site
```
$ hugo new site notes-blog-eissfeldt
Congratulations! Your new Hugo site is created in /ThisIsFake/notes-blog-eissfeldt.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/, or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>/<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.

$ cd tech_notes
```

## Install minimo theme from this guide: 
`https://fillmem.com/post/fast-secured-and-free-static-site/`
```
$ git submodule add https://github.com/MunifTanjim/minimo themes/minimo
$ git submodule init
$ git submodule update
$ cp themes/minimo/exampleSite/config.toml .
```

Now, I'm able to configure the minimo theme, using the config.toml file I coped in this directory
edit config.toml

- `baseurl` = `https://notes.eissfeldt.com`
- `ugly urls = true`

Run the hugo server locally
``` 
$ hugo server -D
```
To verify the server builds and runs, browse to http://localhost:1313/

Use the default theme template from minimo
```
$ cp themes/minimo/archetypes/default.md archetypes/default.md
```

## Create the first post with hugo new command
```
$ hugo new post/first-post.md
```
Next, edit first-post.md with this procedure. 

## Configure npm
In base folder of the hugo site `~/code/notes-blog-eissfeldt/notes-blog-eissfeldt`
```
npm init
```

Add the scripts section for npm run server, npm run build, and npm run deploy
```
{
  "name": "notes-blog-eissfeldt",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "server": "hugo server -w -v",
    "build": "hugo -v",
    "deploy": "aws s3 sync public/ s3://notes-blog-eissfeldt/ --delete"
  },
  "repository": {
    "type": "git",
    "url": "git@github.com:icefelt/notes-blog-eissfeldt.git"
  },
  "author": "Scott Eissfeldt",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/icefelt/notes-blog-eissfeldt"
  },
  "homepage": "https://notes.eissfeldt.com"
}
```


To verify, browse to notes.eissfeldt.com. You should now see this site and no longer the index.html placeholder. 

You should now be able to re-create this blog if something should occur. 

If you're writing over an existing site, please delete the following AWS services previously setup
```
- cloudfront distribution connected to the CNAME notes.eissfeldt.com
- route53 dns for notes.eissfeldt.com 
- AWS certificate manager, delete cert for notes.eissfeldt.com
```

