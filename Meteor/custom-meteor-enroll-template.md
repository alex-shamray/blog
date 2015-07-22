# Custom Meteor enroll template

In my application I want to seed the database with users and send them an enrollment link to activate their account (and choose a password). I also want them to verify/change some profile data.

In my application I'm doing like this

```javascript
this.route('enroll', {
    path: '/enroll-account/:token',
    template: 'enroll_page',
    onBeforeAction: function () {
        Meteor.logout();
        Session.set('_resetPasswordToken', this.params.token);
        this.subscribe('enrolledUser', this.params.token).wait();
    },
    data: function () {
        if (this.ready()) {
            return {
                enrolledUser: Meteor.users.findOne()
            }
        }
    }
})
```

As enrollment url is like this

```
http://www.yoursite.com/enroll-account/hkhk32434kh42hjkhk43
```

when users click on the link they will redirect to this template and you can render your template

In my publication

```javascript
Meteor.publish('enrolledUser', function (token) {
    return Meteor.users.find({"services.password.reset.token": token});
});
```

After taking the password from the user

```javascript
Accounts.resetPassword(token, creds.password, function (e,r) {
    if (e) {
        alert("Sorry we could not reset your password. Please try again.");
    } else {
        alert("Logged In");
        Router.go('/');
    }
});
```

enroll link

```javascript
Accounts.urls.enrollAccount = function (token) {
    return Meteor.absoluteUrl('enroll-account/' + token);
};
```
