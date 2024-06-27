---
layout: post
title: Github Pages with 'Just the docs'
nav_order: 1
---
# Github Pages with 'Just the docs'
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

- TOC
{:toc}
---

## Create a new repo
- click "[use this template]" to create a GitHub repository
- git clone the repo down 

## Generate a personal access token
- set up a [new personal access token] (PAT) under account 'settings'
 - give it a name (the name of the new repo)
 - Pick a token expiration
 - For Repository Access pick 'Only select repositories' and pick the repository you just created
 - Under 'Permissions', expand 'Repository permissions' - you only need 'Read and write' access to'Contents' 
 - make sure you copy and save your PAT in a secure location. You will not be able to access it again
 - The PAT acts as your password. Your username will be your github username

## Configure the instance
- I plan to use the jekyll-feed plug-in, so this is what my 'Gemfile' looks like this -
```yaml
# Gemfile
source 'https://rubygems.org'

gem "jekyll", "~> 4.3.3" # installed by `gem jekyll`
# gem "webrick"        # required when using Ruby >= 3 and Jekyll <= 4.2.2

gem "just-the-docs", "0.8.2" # pinned to the current release
# gem "just-the-docs"        # always download the latest release

gem 'jekyll-feed'
```
- update '\_config.yml'
- create a debug_config.yml based for local debugging
- this is what my config looks like -
```yaml
# _config.yaml
title: curios
description: weird
theme: just-the-docs

url: https://nikmeiser.github.io

plugins:
  - jekyll-feed

aux_links:aux_links:
  nikmeiser.github.io: https://nikmeiser.github.io

color_scheme: light_pink
# Footer "Edit this page on GitHub" link text
gh_edit_link: true # show or hide edit this page link
gh_edit_link_text: "Edit this page on GitHub"
gh_edit_repository: "https://github.com/nikmeiser/blog" # the github URL for your repo
gh_edit_branch: "main" # the branch that your docs is served from
gh_edit_source: docs # the source that your files originate from
gh_edit_view_mode: "tree" # "tree" or "edit" if you want the user to jump into the editor immediately
```
- this is what my debug_config looks like - 
```yaml
# debug_config.yaml
title: curios [Debug]
description: weird
theme: just-the-docs
debug: true
url: http://localhost:4000

plugins:
  - jekyll-feed

aux_links:
  nikmeiser.github.io: https://nikmeiser.github.io

color_scheme: light_pink
# Footer "Edit this page on GitHub" link text
gh_edit_link: true # show or hide edit this page link
gh_edit_link_text: "Edit this page on GitHub"
gh_edit_repository: "https://github.com/nikmeiser/nikmeiser.gitub.io" # the github URL for your repo
gh_edit_branch: "main" # the branch that your docs is served from
gh_edit_source: docs # the source that your files originate from
gh_edit_view_mode: "tree" # "tree" or "edit" if you want the user to jump into the editor immediately
```

## Running the site on your local machine
- At the comand line navigate to your repo and run the following command to install any dependencies -
```
bundle install
```
- then run the following command to serve the pages locally -
```
bundle exec jekyll serve --config debug_config.yml
```
- Not all changes are picked up and you may need to restart the local server if you don't see your changes
- Don't forget to copy any config changes from the debug_config to config.yml

## Customizing your implementation
- I have made two customizations to my site - 
 - Made the side naviation column narrower
 - Changed the color of the links to pink
- I did this by overriding the 'Just-the-docs' default CSS
- I added a `_sass` directory to my project root and created a `color_schemes` directory and a `custom` directory underneath it
- the `light_pink.scss` override uses the 'light' theme that ships with 'Just-the-docs and only changes the link color - 
```scss
@import "./color_schemes/light";
$pink-000: #cd0c99;
$link-color: $pink-000;
```
- the sidebar customizations look like this -
```scss

.side-bar {
    z-index: 0;
    display: flex;
    flex-wrap: wrap;
    background-color: $sidebar-color;

    @include mq(md) {
	flex-wrap: nowrap;
	position: fixed;
	width: $nav-width-md;
	height: 100%;
	flex-direction: column;
	border-right: $border $border-color;
	align-items: flex-start;
    }

    @include mq(lg) {
	width: calc((100% - #{$nav-width + $content-width}) / 2);
	min-width: $nav-width;
    }
}

.site-nav {
    width: 100%;
}

```
- you can find more customization options on the [Just-the-docs site]
- you will need to add the directories for the components you override

## Setting up your Github pages deployment
- on Github navigate to your repo and click on 'Settings'
- select 'pages' on the left
- under 'source', select `Github Actions` instead of 'Deploy from a branch'. This will bring up a `Configure` button for Jekyll
- click on `Configure` and save the default values. You will be prompted to commit your changes to the `main` branch
- the workflow shoudl kick off automatically. You can see the progress under the `Actions` tabe of your repo
> [!NOTE]
> remember to perform a `git pull` on your local machine after savig the Jekyll config or you will run into a merge conflict the next time you push a change
- look for status under 'actions' and fix any errors
## Directory structure
- this is what my directory looks like - 
```
nikmeiser.github.io
├── assets
├── _config.yml
├── debug_config.yml
├── Gemfile
├── Gemfile.lock
├── index.md
├── LICENSE
├── _posts
│   └── 2024-06-27-github-pages-with-jtd.md
├── README.md
└── _sass
    ├── color_schemes
    │   └── light_pink.scss
    └── custom
        └── custom.scss

```
## Common errors

> did not find expected key while parsing a block mapping at line 1 column 1 (Psych::SyntaxError)
-  This error  means that your config file probably has a missing quote and can't be parsed


[use this template]: https://github.com/just-the-docs/just-the-docs-template/generate
[new personal access token]: https://github.com/settings/personal-access-tokens/new
[Just-the-docs site]: https://just-the-docs.github.io/just-the-docs/docs/customization/