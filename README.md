# S3 as CDN

This repo demonstrates how to serve static websites using an S3 compatible backend (min.io).

## Installing

`docker-compose up`

1. Create a `static` bucket
2. Create a `blog.windmill` static subfolder & push inside an index.html file
3. Create a `staging.blog.windmill` static subfolder & push inside another index.html file
4. Create a `feature-new-page.blog.windmill` static subfolder & push inside another index.html file

Add the previously domain names to your `/etc/hosts` file

`mc policy set download minio1/static`

restart the containers & try to access to blog.windmill, staging.blog.windmill through your web browser

> The nginx configuration & settings are heavily inspired from this gist: https://gist.github.com/harshavardhana/f05b60fe6f96803743f38bea4b565bbf

# What it resolves

This proof of concept allows anybody to deploy static websites on an internal network S3 backend by just creating a bucket whose name is simply the website. Those websites may be SPAs.

It is a convenient solution to problems where people are asking how to deploy a SPA into a docker container; in the the best solution is maybe not to do so & just publish your SPA dist/ folder to the according S3 bucket when your app gets merged into your production branch/tagged.

## A workflow use-case

You're having a blog (blog.windmill) which is under heavy development. You have three kinds of environments: 

* production
* staging
* features

When you merge something into master, it must be deployed to blog.windmill.
When you merge something into develop, it must be deployed to staging.blog.windmill.
When you push something into a feature-* branch, it must be deployed to feature-my-feature.blog.windmill.

You can imagine a process where you create the buckets if they do not exists yet, then push inside the dist/ content of your SPA.

The nginx server will serve any static content as long as it is hosted inside the S3 static bucket.
