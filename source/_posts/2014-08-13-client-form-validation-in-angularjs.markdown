---
layout: post
title: "client form validation in angularjs"
date: 2014-08-13 09:33:35 +0800
comments: true
categories: 
---

angularjs has built-in helper to validate form in client side, here is how you can use it.


Your html:

```

<form name="myForm" ng-submit="submitForm(myForm)">
  <div>
    <input type="email" ng-model="user.email" name="uEmail" required />
    <div ng-show="myForm.uEmail.$dirty && myForm.uEmail.$invalid">Invalid:
      <span ng-show="form.uEmail.$error.required">Tell us your email.</span>
      <span ng-show="form.uEmail.$error.email">This is not a valid email.</span>
    </div>
  </div>
</form>

```

Notice that the name in form and input fields which is used as referenced when pass form object and check whether the field is valid.

$dirty use to check whether the field value changed and $invalid to check whether the field in valid, these two methods are useful to show errors message for the fields.

Your controller: 

``` coffeescript

angular.module('app').controller('SampleController', [

  '$scope', ($scope)->
    
    $scope.submitForm = (form)->
      if form.$valid
        console.log 'submit the form to server'

])

```

In controller, we can use the form object to check whether the form is valid before submit data to server.

There's a lot of ways that you can valid your input fields.

* required
* ng-minlength
* ng-max-length
* ng-pattern (use regular expression for validation)
* email ( if you specify email as type )
* number ( if you specify number as type )
* url ( if you specify url as type )
* custom validation


Reference url: 

http://www.ng-newsletter.com/posts/validations.html
http://scotch.io/tutorials/javascript/angularjs-form-validation

