# chat_Agular_Firebase

#Agregar código HTML
  ```
  <html>

<head>
  <title>Awesome chat app</title>
</head>

<body ng-app="chatapp" ng-controller="MainCtrl as app">
  <div class="top-bar">
    <div class="top-bar-left">
      <ul class="menu">
        <li class="menu-text">{{ app.name }}</li>
      </ul>
    </div>
    <div class="top-bar-right">
      
      <ul class="menu" ng-show="app.auth.loggedIn">
        <li class="menu-text"><em>Say something {{ app.auth.name }}!</em></li>
      </ul>
    </div>
  </div>


  <div class="row" ng-hide="app.auth.loggedIn">
    <div class="small-12 medium-6 medium-offset-3 columns">
      <hr class="invisible" />
      <form ng-submit="app.auth.logInUser()" ng-submit="app.auth.logInUser()" name="loginForm" class="callout">
        <h2>Log in to chat!</h2>
        <p>
          <label for="">Your name</label>
          <input type="text" ng-model="app.auth.name" required />
        </p>
        <p><button class="button expanded" type="submit">Log In</button></p>
      </form>
    </div>
  </div>
  <div class="row" ng-show="app.auth.loggedIn">
    <div class="small-12 medium-6 medium-offset-3 end columns">
      <ul class="messages no-bullet" message-box>
        <li ng-repeat="message in app.messages.data">
          <div class="callout">
            <div class="row">
              <div class="small-1 columns">
                <span class="label radius">
                  {{ message.name | limitTo:1 }}
                </span>
              </div>
              <div class="small-11 columns">
                <strong>{{message.name}}</strong>
                <p>{{message.text}}</p>
              </div>
            </div>
          </div>
        </li>
      </ul>
      <form class="write-box">
        <div class="row collapse postfix">
          <div class="small-9 columns">
            <input type="text" ng-model="app.messages.newMessage" ng-disabled="!app.auth.loggedIn" />
          </div>
          <div class="small-3 columns">
            <button class="button expanded" ng-click="app.messages.add(app.messages.newMessage)">Send</button>
          </div>
      </form>
      </div>
    </div>
  </div>
</body>

</html>
  ```
  
  # Agregar el archivo app.js
  ```
  
  ```
