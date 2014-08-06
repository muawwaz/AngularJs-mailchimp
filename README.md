AngularJs-mailchimp
===================

Angular controller to automate MailChimp subscriptions.
# Installation

Add a `<script>` to your `index.html`:

```html
<script src="/bower_components/angularjs-mailchimp/angularjs-mailchimp.js"></script>
```

And add `mailchimp` as a dependency for your app:

```javascript
angular.module('myApp', ['mailchimp']);
```

# Justification of Method

Instead of relying on a resource or a service, I chose to make this module as reusable as possible. This means that all configuration is done in the [HTML shown below]. So, you can have one page with as many subscription forms as you'd like as long as you [configure] everything properly.

# Configuration

1. Log into your Mailchimp Account
2. Click on "Lists" from the sidebar
3. Click on the list for which you want
4. Click "Signup forms"
5. Select "Embedded forms"
6. Select "Naked"

You must obtain the following information from the form action attribute code:

* `mailchimp.username` - Your MailChimp username, immediately follows `http://`
* `mailchimp.dc` - Your MailChimp distribution center, immediately follows your username
* `mailchimp.u` - Unique string that identifies your account. It is obtained from 
* `mailchimp.id` - A unique string that identifies your list.

Example::

    <form action="http://username.us1.list-manage.com/subscribe/post?u=a1b2c3d4e5f6g7h8i9j0&amp;id=aabb12" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>

Result:

* `mailchimp.username` - `username`
* `mailchimp.dc` - `us1`
* `mailchimp.u` - `a1b2c3d4e5f6g7h8i9j0`
* `mailchimp.id` - `aabb12`

# Usage

The results from the last step will be populated in your form via `ng-init`.

So, add a `<form>` to your template as follows and replace the values with
those you obtained:

```html
<form name="MailchimpSubscriptionForm" ng-controller="MailchimpSubscriptionCtrl">
  <div ng-hide="mailchimp.result === 'success'">
    <input class="hidden" type="hidden" ng-model="mailchimp.username" ng-init="mailchimp.username='username'">
    <input class="hidden" type="hidden" ng-model="mailchimp.dc" ng-init="mailchimp.dc='us1'">
    <input class="hidden" type="hidden" ng-model="mailchimp.u" ng-init="mailchimp.u='a1b2c3d4e5f6g7h8i9j0'">
    <input class="hidden" type="hidden" ng-model="mailchimp.id" ng-init="mailchimp.id='aabb12'">
    <input type="text" name="fname" ng-model="mailchimp.name" placeholder="Enter Full Name">
    <input type="email" name="email" ng-model="mailchimp.email" placeholder="Email address" required>
    <button ng-disabled="MailchimpSubscriptionForm.$invalid" ng-click="addSubscription(mailchimp)">Join</button>
    </div>
  </div>

  <!-- Show error message if MailChimp unsuccessfully added the email to the list. -->
  <div ng-show="mailchimp.result === 'error'" ng-cloak>
    <span ng-bind-html="mailchimp.errorMessage"></span>
  </div>

  <!-- Show success message if MailChimp returned successfully. -->
  <div ng-show="mailchimp.result === 'success'" ng-cloak>
    <span ng-bind-html="mailchimp.successMessage"></span>
  </div>
</form>
```

# Rights

This is a updated working Version of https://github.com/keithio/angular-mailchimp/
