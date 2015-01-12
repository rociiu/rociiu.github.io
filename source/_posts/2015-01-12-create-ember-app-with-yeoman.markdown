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


* * *

#### References:

* [http://nodejs.org](http://nodejs.org)
* [https://www.npmjs.com](https://www.npmjs.com)
* [http://yeoman.io](http://yeoman.io)
* [http://gruntjs.com](http://gruntjs.com)
* [http://emberjs.com](http://emberjs.com)
* [http://bower.io](http://bower.io)

