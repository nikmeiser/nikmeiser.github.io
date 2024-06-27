---
layout: default
parent: ELI5
title: Github Pages with 'Just the docs'
nav_order: 1
---
# Github Pages with 'Just the docs'

## Create a new repo
- click "[use this template]" to create a GitHub repository
- git clone the repo down 

## Generate a personal access token
- set up a [new personal access token] under account 'settings'
 - give it a name (the name of the new repo)
 - Pick a token expiration
 - For Repository Access pick 'Only select repositories' and pick the repository you just created
 - Under 'Permissions', expand 'Repository permissions' - you only need 'Read and write' access to'Contents' 
 - login with your username and PAT

## Configure the instance
- update '\_config.yml'
- push changes
- go to the rep settings and set up the github pages deployment under 'pages'
- under 'pages', change the 'source' to 'github actions' and commit your changes (don't forget to pull your changes down)
- look for status under 'actions' and fix errors


[use this template]: https://github.com/just-the-docs/just-the-docs-template/generate
[new personal access token]: https://github.com/settings/personal-access-tokens/new
