# Respublica.js

Respublica is a framework based on *Vieux UI development architecture*. For more information, visit [vieux](http://github.com/vieuxjs/vieux) page.

## Overview

```javascript
function AJAXSatellite() {
  
}

function LoginRegime() {
  
}

function LoginUndertaker() {

}

function LoginCulture() {

}

function LoginRepresentative() {

}

function LoginDiplomat(regime) {
  
}

respublica.
  satellite(AJAXSatellite).
  regime(LoginRegime).
  undertaker(LoginUndertaker)



var LoginRegime = respublica.Regime({
  onRise: function () {
    // on regime rise
  },
  doLogin: function (username, password) {
    var user = new LoginStereotype({
      username: username,
      password: password
    });
    LoginUndertaker.doLogin(user).then(this.onLogin);
  },
  onLogin: function () {
    // Publish message to representatives
    this.publish('loggedIn', true);
  }
});

var AJAXSatellite = respublica.Satellite({
  onLaunch: function () {
    // on satellite launched.
  },
  post: function () {
    // impl. ajax post.
  }
});

var LoginUndertaker = LoginRegime.Undertaker({
  doLogin: function (user) {
    return AJAXSatellite.post('/login', user).then(this.done);
  }
});

var LoginCulture = LoginRegime.Culture({
  onRise: function () {
    this.territory.on('button[type="submit"]', 'click', this.doLogin);
    
    this.representative.on('loggedIn', this.territory.hide);
  },
  
  doLogin: function () {
    var user = this.territory.$('form').values(); // {username, password}
    this.representative.doLogin(user);
  }
  
  onFall: function () {
    // on culture fall
  }
});

var LoginRepresentative = LoginCulture.Representative({
  onAppoint: function () {
    this.regime.listen('loggedIn', this.publish); // forward regime's message to culture
  },
  doLogin: function (user) {
    this.regime.doLogin(user.username, user.password);
  }
});

var LoginDiplomat = LoginRegime.Diplomat({
  onAppoint: function () {
    this.regime.listen('loggedIn', this.onLogin);
  },
  onLogin: function (data) {
    // Say popup diplomat login status.
    PopupDiplomat.message('loggedIn', data);
  }
});
```

## Integration

**Simpler:**
```html
<script src="respublica.js"></script>
<script>
var MyRegime = respublica.Regime({
  // ...
});
</script>
```

**Advanced:** *(for the people who love Browserify, Webpack, etc.)*
```javascript
var respublica = require("respublica");
var MyRegime = respublica.Regime({
  // ...
});
```

