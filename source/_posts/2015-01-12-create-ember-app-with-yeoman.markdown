---
layout: post
title: "create ember app with yeoman"
date: 2015-01-12 08:28:29 +0800
comments: true
categories: 
---

### 1. install dependencies

Download and install from [nodejs](http://nodejs.org), it include NPM for node package manage

Install yeoman, grunt, bower.

    npm install -g yo grunt-cli bower

### 2. install ember generator

Run yo command, you will see options for execute yo commands:

    yo

Use your up/down keys to select different menu/command to execute, select install generator.

    ❯ Install a generator

It will prompt:
  
    ? Search npm for generators:

Enter ember to search emberjs related generator. after a while it will return search results for 'ember':

    ? Here's what I found. Official generator → ෴
      Install one? (Use arrow keys)
    ❯ ember ෴
      ember-plugin
      rjs-ember
      ember-plus
      base
    
Select ember marked as '෴' and press enter to install it.

### 3. use yo command to create the app

First you need to create a project folder, you can create with command:

    mkdir myapp
    yo

After run 'yo', it will give options to run generator, select Ember (use up/down arrow keys):

      Run a generator
      Angular
     ❯Ember
      Mocha
      Karma

Press enter, after a while it will ask whether to include Bootstrap, feel free to select Y or n. After that you'll see something like this:

     create .gitignore
     create .gitattributes
     create .bowerrc
     create bower.json
     create package.json
     create .jshintrc
     create .editorconfig
     create Gruntfile.js
     create app/templates/application.hbs
     create app/templates/index.hbs
     create app/index.html
     create app/styles/style.scss
     create app/scripts/app.js
     create app/scripts/store.js
     create app/scripts/router.js
     create app/scripts/routes/application_route.js

      I'm all done. Running bower install & npm install for you to install the required dependencies. If this fails, try running the command yourself.
      ....

It will install all js/libraries dependencies with: bower install & npm install. Finally you will see ascii art like:

         _-----_
        |       |    .----------------------.
        |--(o)--|    |     Bye from us!     |
       `---------´   |      Chat soon.      |
        ( _´U`_ )    |                      |
        /___A___\    |      Yeoman team     |
         |  ~  |     |   http://yeoman.io   |
       __'.___.'__   '----------------------'
     ´   `  |° ´ Y `

Yeah!

### 4. run grunt to build the app

Run grant command to build the project.

    grunt

### 5. run grunt serve to preview and edit the app

    grunt serve

### 6. deploy the app


after edit and create some awesome features, you definitely want to deploy the app, right? Here I'll show you how to deploy the app in [divshot.com](https://divshot.com) static web hosting for developers.

First you need to register a account in [divshot.com](https://divshot.com), after register you need to install npm package for running divshot command

    npm install -g divshot-cli

Then use divshot command to login your account:

    divshot login

After that you can go to your project directory, run 'divshot init' command:

    divshot init
    name: (myapp) rociiu-ember-demo
    root directory: (current) dist
    clean urls: (y/n) y
    error page: (error.html) n
    Would you like to create a Divshot.io app from this app?: (y/n) y
    Creating app ...

After the steps, it will create a configuration file(divshot.json) inside the root directory. Then deploy the app with 'divshot push':

    divshot push

    Creating build ...  App does not yet exist. Creating app rociiu-ember-demo ... ✔
    Hashing Directory Contents ... ✔

    Syncing 15 files: [==================================================] 100%


    Finalizing build ... ✔
    Releasing build to development ... ✔

    Application deployed to development
    You can view your app at: http://development.rociiu-ember-demo.divshot.io

Your app should be live now.

* * *

#### References:

* [http://nodejs.org](http://nodejs.org)
* [https://www.npmjs.com](https://www.npmjs.com)
* [http://yeoman.io](http://yeoman.io)
* [http://gruntjs.com](http://gruntjs.com)
* [http://emberjs.com](http://emberjs.com)
* [http://bower.io](http://bower.io)

