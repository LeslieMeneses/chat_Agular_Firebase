# chat_Agular_Firebase

# Archivos
  |__app.js
  |__index.html

#Agregar código HTML index.html
  ```
<html>

  <head>
    <!-- Incluir librerias -->
    <title>Awesome chat app</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.2/angular.min.js"></script>
    <script src="https://cdn.firebase.com/js/client/2.2.4/firebase.js"></script>
    <script src="https://cdn.firebase.com/libs/angularfire/1.2.0/angularfire.min.js"></script>
    <script src="app.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/foundation/6.2.0/foundation.min.css">
  </head>

  <!-- Agregar el controlador al template -->
  <body ng-app="chatapp" ng-controller="MainCtrl as app">
    <div class="top-bar">
      <div class="top-bar-left">
        <ul class="menu">
          <!-- modelo (app.name) -->
          <li class="menu-text">{{ app.name }}</li>
        </ul>
      </div>
      <div class="top-bar-right">
        
        <ul class="menu" ng-show="app.auth.loggedIn">
          <li class="menu-text"><em>Say something {{ app.auth.name }}!</em></li>
        </ul>
      </div>
    </div>

    <!-- llama a la funcion que hace la coneccion con FIREBASE -->
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

    <!-- Lista de chats -->
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
                  <p>{{message.time |  date : format : timezone}}</p>
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
  
#Agregar el archivo app.js
  ```
// Modulo de la aplicacion llamado chatapp
angular.module('chatapp', ['firebase'])
.controller('MainCtrl', MainCtrl)
.service('Auth', AuthService)
.service('Messages', MessagesService)
.directive('messageBox', MessageBoxDirective)
.constant('FirebaseRef', new Firebase('https://first-853f5.firebaseio.com/messages'));

function MainCtrl(Auth, Messages){
  var vm = this;
  vm.name = 'Funky Awesome Chat App';
  
  vm.auth = Auth;
  vm.messages = Messages;
}

// Conneccion con firebase
function AuthService($rootScope){
  
  var Auth = {
    name: '',
    loggedIn: false
  };
  
  Auth.logInUser = function(){
    this.loggedIn = true;
    $rootScope.$broadcast('loggedIn');
  };
  
  return Auth;
}

// Obtiene los mensajes de firebase
function MessagesService(Auth, $rootScope, $firebaseArray, FirebaseRef){
  var Messages = {
    data: $firebaseArray(FirebaseRef)
  };
  
  Messages.add = function(message){
    Messages.data.$add({ 
      text: message,
      name: Auth.name,
      time: Date.now()
    });
    
    delete Messages.newMessage;
    $rootScope.$broadcast('newMessage');
  }
  
  return Messages;
}

// Directiva para la caja de mensajes
function MessageBoxDirective($rootScope, $timeout){
  var MessageBox = {
    restrict: 'A',
    link: function(scope, el, attrs){
      
      function scrollToBottom(){
        $timeout(function(){
          el[0].scrollTop = 9999;
        }, 0);
      }
      
      $rootScope.$on('loggedIn', scrollToBottom);
      $rootScope.$on('newMessage', scrollToBottom);
      
    }
  }

  return MessageBox;
}
  ```
