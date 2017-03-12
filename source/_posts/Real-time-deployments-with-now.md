---
title: Real time deployments with Zeit's Now
tags:
  - JavaScript
  - DevOps
  - Deployment
categories:
  - DevOps
date: 2017-03-12 17:00:00
---


This time I want to share with you an amazing project made by [Zeit](https://zeit.co/), the creators of [socket.io](http://socket.io/)
and [mongoose](http://mongoosejs.com/).
[Now](https://zeit.co/now) will deploy your JavaScript (Node.js), Docker or static projects to the cloud at the speed of light. Almost all you have to do is execute one single command in the console and you will get a URL where you can see your deployed project.

I will only show how it works with a Node.js App. But the same principles apply for Docker and static projects.

You have two ways to use Now:

- **CLI tool** (install with `npm i -g now`)
- **Desktop version** (only MacOS), available for download [here](https://zeit.co/download#). It also includes the CLI tool.

After successfully logging into your account, you are ready to deploy "whatever" you want in seconds.

Logging-in with the CLI:

```bash
> now --login
> Enter your email: xxx@xxx.com
> Please follow the link sent to xxx@xxx.com to log in.
> Verify that the provided security code in the email matches Xxx Xxxxx.

✔ Confirmed email address!

> Logged in successfully. Token saved in ~/.now.json
```

Desktop version welcome page:
![Desktop app](desktop-1.png)
![Desktop app](desktop-2.png)

## Deploy a simple Express App

For the sake of showing how Now works, I will create a very simple Express App.

index.js:

```javascript
'use strict';

const app  = require("express")();
const port = process.env.PORT || 4201;

 app.get("/", function(req, res) {
    res.send(`<h2>Express server is up and running on port ${port}</h2>`);
    console.info(`Route '/' was called`)
 });

 app.listen(port, function() {
   console.log("Express app listening on port:", port);
 });
```

Note: it is mandatory to have an npm *start* or *now-start* script defined in your package.json in order to deploy. For example:

```json
...
"scripts": {
    "start": "node index.js",
    "now-start": "node index.js" // This script will overwrite "start"
  }
...
```

(For *npm build* you can also have a specific now-build script. The same principle as above applies)

Now let's deploy!
Here, again you can use the CLI or drag&drop your folder to the Desktop application.

That's how the CLI tool looks like:

```bash
> now
> Deploying ~/Desktop/zeit-now-test
> Using Node.js 7.6.0 (default)
> Ready! https://zeit-now-test-gedqsinqpr.now.sh (copied to clipboard) [2s]
> Upload [====================] 100% 0.0s
> Sync complete (1.07kB) [4s]
> Initializing…
> Building
> ▲ npm install
> ⧗ Installing:
>  ‣ express@^4.15.2
> ✓ Installed 42 modules [2s]
> ▲ npm start
> > zeit-now-test@1.0.0 start /home/nowuser/src
> > node index.js
> Express app listening on port: 4201
> Deployment complete!
```

Now the Express App is publicly available under `https://zeit-now-test-gedqsinqpr.now.sh`.
And it took only about 2 seconds. Impressive!


## Using custom deployment names

For each new deployment with now, a unique URL will be generated. Note: The old ones always remain available though until you delete them.

But maybe you do not want to have a new URL each time you deploy your project... if that's the case, Now has you covered.
For that, you will need to create an alias:

```bash
> now alias https://zeit-now-test-gedqsinqpr.now.sh zeit-now-test
> Success! Alias created (DPwARUkOxzcde7hXCoUgBl1F):
https://zeit-now-test.now.sh now points to https://zeit-now-test-gedqsinqpr.now.sh (OheF43olzda9C1GjuWnoVTuf) [copied to clipboard]
```

The first parameter has to be a URL of an already existing Now deployment. You can check the list of deployments with `now ls`.
The second parameter is the custom name of the alias, so it generates the URL `https://zeit-now-test.now.sh`.

After each new deployment you can update the alias repeating the command above with the newest deployment URL.

With the premium version (which costs $14.99/mo) you can even point your deployment to a custom domain.
(Some domains as `*.de` are not working yet though).


## View the source code of the project

There's also the possibility to view the source code of the projects directly from the browser, adding `/_src` to the URL:
e.g. `https://zeit-now-test.now.sh/_src`.

![Source code](source-code.png)

If you have a premium version you can set your projects to `private` and the source code will not be shown.
For the free version all the project are by default `public`.

## Use environmental variables

You will probably also want to use environmental variables to store some secret information. Now also covers this and lets you work with
environmental variables.

Let's say I want to have a variable called `SALUTATION` and use it in the Express App we created before.

```javascript
'use strict';

const app  = require("express")();
const port = process.env.PORT || 4201;

let salutation = process.env.SALUTATION; // our new variable

 app.get("/", function(req, res) {
    res.send(`
    <h1>Hello, ${salutation}!</h1>
    <h2>Express server is up and running on port ${port}</h2>
    `);
    console.info(`Route '/' was called`)
 });

 app.listen(port, function() {
   console.log("Express app listening on port:", port);
 });
```

There are three possible ways to do that:

#### Passing them manually upon deployment

```bash
> now -e SALUTATION=folks
...
> Ready! https://zeit-now-test-vwnmjgkefe.now.sh (copied to clipboard) [1s]
...
> Deployment complete!
```

Resulting in:
![Env Variables](env-var-result-1.png)


#### Storing them in package.json, within an npm script

```json
...
 "scripts": {
    "start": "node index.js",
    "deploy-to-now": "now -e SALUTATION=people"
  },
...
```

And then run the script:

```bash
> npm run deploy-to-now

> zeit-now-test@1.0.0 deploy-to-now /Users/oleg.korol/Desktop/zeit-now-test
> now -e SALUTATION=people

...
> Ready! https://zeit-now-test-idfdrtjbeg.now.sh (copied to clipboard) [1s]
...
> Deployment complete!
```

The result is:
![Env Variables 2](env-var-2.png)

#### The other, and best way is to use `secrets`

Of course you do not want to push sensitive data directly to your git repository so that everyone can see it. So if the environmental
variables are storing any secret data you should only use this way for storing them.

To create a new secret just run:

```bash
> now secret add salutation 'ladies and gentlemen'
> Success! Secret salutation (sec_n3R4TrxIgib5C67896bXehjnmZR) added [981ms]
```

Then just adapt your npm script to use the new secret (`@salutation`):

```json
...
"scripts": {
    "start": "node index.js",
    "deploy-to-now": "now -e SALUTATION=@salutation"
  },
...
```

... and execute `npm run deploy-to-now`.

Voila!
![Voila!](voila.png)


## Conclusion

Now is a tool that has the potential to exponentially increase the workflow speed of any development team, by providing easy and blazing fast deployments. I really enjoyed playing around with it and cannot wait to implement it in real projects.

Try it out and feel free to leave your comments below!






