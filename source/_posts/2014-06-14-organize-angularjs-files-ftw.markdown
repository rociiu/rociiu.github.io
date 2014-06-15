---
layout: post
title: "organize angularjs files ftw"
date: 2014-06-14 19:47:50 +0800
comments: true
categories: anguarljs
public: true
---

In a recent project I just started to use angularjs to develop complicated front UI, this project is not a SPA, we only use angularjs in places that ui elements is so complicated that is easier to do with front end. Since new on angularjs, i searched on the internet for best practices that organize large codebase and module naming, found couple ways.

### 1. Put everything in one file for simplest case.
  
``` coffeescript app.js.coffee everything in one file
  
  angularjs
    .module('myApp', [])
    .controller('MyCtrl', ['$scope', ($scope)->
      $scope.hello = "world"
    ])
    .constant('Contacts', ['Foo', 'Bar'])
    ...

```

### 2. Put different kind of stuff in different files

``` coffeescript app.js.coffee define app namespace
  
  angularjs.
    .module('myApp', [])
  
```

``` coffeescript controllers.js.coffee define controllers
  
  angularjs.module('myApp')
    .controller('OneCtrl', ['$scope', ($scope)->
      $scope.hello = 'world'
    ])
    .controller('TwoCtrl', ['$scope', ($scope)->
      $scope.foo = 'bar'
    ])

```

``` coffeescript constants.js.coffee define constants
  
  angularjs.module('myApp')
    .constant('Contacts', ['Foo', 'Bar'])

```

### 3. Create directories to save different kind of files

```
  - app.js.coffee
  controllers/
    OneCtrl.js.coffee
    TwoCtrl.js.coffee
  constants/
    Contants.js.coffee
    Contacts.js.coffee
  
```

### 4. Create features directories to store everything for each features


```
  - app.js.coffee
  Users
    - UserCtrl.js.coffee
    - UserModel.js.coffee
  Contacts
    - ContactsCtrl.js.coffee
    - ContactModel.js.coffee

```

### 5. How do organize code for multiple angularjs apps

```
  - app.js.coffee
  - modules
    AppOne.js.coffee
    AppTwo.js.coffee
  - controllers
    AppOne/
      OneCtrl.js.coffee
    AppTwo/
      TwoCtrl.js.coffee
  - models
    AppOne/
      UserModel.js.coffee
    AppTwo/
      ContactModel.js.coffee

```

In app.js.coffee, I define top level module to include basic dependencies share between apps.

``` coffeescript app.js.coffee
  angularjs.module('app', ['dep1', 'dep2'])
```

In AppOne.js.coffe, it define a new module and load app as dependencies and the shared dependencies define in app will be available in AppOne.

``` coffeescript AppOne.js.coffee
  angularjs.module('app.appOne', ['app'])
```

In html only need to specify AppOne in place that app one is need.

``` html
  <div ng-app='app.appOne'>
    <div ng-controller="OneCtrl">
    </div>
  </div>
```

### Conculsion

I didn't choose solution 4 because it seems not good to put different stuff together even though it's easier to find files for changes. So I follow solution 3 and extend it for multiple apps.

