---
title: Hexo + GitHub Pages + Cloudflare
tags:
  - Hexo
  - GitHub
  - JavaScript
  - Cloudflare
  - Hosting
  - Blogging
categories:
  - Hosting
  - Blogging
  - CDN
header-img: ./post-bg.jpg
date: 2017-02-05 17:46:56
---


In this post I will explain how to create a blog (or any other kind of website) using a static site generator called [Hexo](https://github.com/hexojs/hexo), completely built on JavaScript.
The source code will be on GitHub and will be deployed to GitHub Pages.

#### Configure GitHub Pages
First of all you will need a GitHub repository where you are going to store the source code for your site and later deploy it to GitHub Pages.
For those who did not hear about it yet:
> GitHub Pages is a static site hosting service.
  GitHub Pages is designed to host your personal, organization, or project pages directly from a GitHub repository.<sup id="f1">[1](#foot1)</sup>
  
You can find a great and short step-by-step guide on how to set up your repository [here](https://pages.github.com/).

Note that for user and organization pages, GitHub will use the content inside the `master` branch for the deployment. For project pages, GitHub normally uses the branch `gh-pages`.

You can try to commit a dummy `index.html` file to your repository and then go to `username.github.io` from your browser. You should now see the contents of that file.

Once here, you should already have your repository set up and a working GitHub page. Cool!
Now let's move to the next step.

#### Let's get started with Hexo
As I was saying before, Hexo is a simple, blazing fast static site generator (or blog framework, as their creators call it) that runs entirely on Node.js.
You just need to grab a theme (or create one) and start writing a post using Markdown. Then Hexo generates the static files and deploys them to GitHub Pages or Heroku for you. Just like that!

Here is a complete [guide](https://hexo.io/docs/) on how to install Hexo on your machine.

If you already have Node.js and Git running, all you need to do is install it globally with npm: 
`npm install -g hexo-cli`

Then you will need to configure it and you are ready to start writing your amazing content!
I will not go through all the steps (which are actually not so many) to setup your Hexo project. You can find [here](https://hexo.io/docs/setup.html) a short setup guide.

After that, when creating your content, you will be using four commands most of the time: 
* `hexo clean`, to delete the database (which is a JSON file) and the public folder.
* `hexo generate`, to generate a new public folder with the updated contents.
* `hexo serve`, to initiate a local instance running on your `localhost`.
* `hexo deploy`, to deploy the public folder to your platform of choice. In this case to GitHub Pages.

Note that the command `hexo deploy` uses your deployment configuration inside the `_config.yml` file. It looks like this:
```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/username/username.github.io
  branch: master # remember that GitHub Pages uses this branch 
```

### Use a custom domain for your site
If you have your own domain (I have purchased one from [GoDaddy](https://uk.godaddy.com/)) it is also possible to point it to your GitHub page. It just takes a couple of steps.

You can find some detailed instructions for apex domains [here](https://help.github.com/articles/about-supported-custom-domains/#setting-up-apex-domains), or for subdomains [here](https://help.github.com/articles/setting-up-a-custom-subdomain/).

This process is always different depending on your domain registrar.

#### Optional: Use a CDN
For all my websites I always like to configure a CDN (Content Delivery Network), which gives you some extras, like: SSL certificate generation, smart error-handling, analytics, website caching and advanced security features, among others.

For this purpose I can totally recommend [Cloudflare](https://www.cloudflare.com/de/). The guys are doing a really great job and it doesn't cost you a dime for small projects.


You are now all set. Go and write some amazing content to share with the world!
* * *

If there's is any question I can help you with, please comment below.
Feedback is always welcome, and I would like to know what you think about Hexo and GitHub Pages.

Stay tuned for more!



 References:
<a name="foot1" style="none">1</a>: https://help.github.com/articles/what-is-github-pages/ [↩](#f1)

