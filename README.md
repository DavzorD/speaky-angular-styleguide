# Speaky - Angular Style Guide

Inspired by https://github.com/johnpapa/angular-styleguide and God.

## Variable names

Always use clear and self-explanatory variable names.

```javascript
<!-- avoid -->
var displayNavbar = ns.display
```
```javascript
<!-- recommended -->
var isDiplayedNavbar = navbarService.isDisplay
```


Logique dans les services
Variable avec nom complet ($scope.navigationService = navigationService et pas $scope.ns)
Retirer les console.log et mettre des $log.debug
Insérer $log dans chaque controller, service

les box d’help ou trucs visibles qu’on peut show/hide s’appellent 
vm.isDisplayed.helpMessage (par exemple si c’est un help message), ou bien vm.isDisplayed.helpSelectAMeeting, …


## Controllers

controllerAs View Syntax

[Style Y030]

Use the controllerAs syntax over the classic controller with $scope syntax.

Why?: Controllers are constructed, "newed" up, and provide a single new instance, and the controllerAs syntax is closer to that of a JavaScript constructor than the classic $scope syntax.

Why?: It promotes the use of binding to a "dotted" object in the View (e.g. customer.name instead of name), which is more contextual, easier to read, and avoids any reference issues that may occur without "dotting".

Why?: Helps avoid using $parent calls in Views with nested controllers.
```javascript
<!-- avoid -->
<div ng-controller="Customer">
    {{ name }}
</div>
```

```javascript
<!-- recommended -->
<div ng-controller="Customer as customer">
    {{ customer.name }}
</div>
controllerAs Controller Syntax
```
[Style Y031]

Use the controllerAs syntax over the classic controller with $scope syntax.

The controllerAs syntax uses this inside controllers which gets bound to $scope

Why?: controllerAs is syntactic sugar over $scope. You can still bind to the View and still access $scope methods.

Why?: Helps avoid the temptation of using $scope methods inside a controller when it may otherwise be better to avoid them or move them to a factory. Consider using $scope in a factory, or if in a controller just when needed. For example when publishing and subscribing events using $emit, $broadcast, or $on consider moving these uses to a factory and invoke from the controller.

/* avoid */
function Customer($scope) {
    $scope.name = {};
    $scope.sendMessage = function() { };
}
/* recommended - but see next section */
function Customer() {
    this.name = {};
    this.sendMessage = function() { };
}
controllerAs with vm

[Style Y032]

Use a capture variable for this when using the controllerAs syntax. Choose a consistent variable name such as vm, which stands for ViewModel.

Why?: The this keyword is contextual and when used within a function inside a controller may change its context. Capturing the context of this avoids encountering this problem.

/* avoid */
function Customer() {
    this.name = {};
    this.sendMessage = function() { };
}
/* recommended */
function Customer() {
    var vm = this;
    vm.name = {};
    vm.sendMessage = function() { };
}
Note: You can avoid any jshint warnings by placing the comment above the line of code. However it is not needed when the function is named using UpperCasing, as this convention means it is a constructor function, which is what a controller is in Angular.

/* jshint validthis: true */
var vm = this;
Note: When creating watches in a controller using controller as, you can watch the vm.* member using the following syntax. (Create watches with caution as they add more load to the digest cycle.)

<input ng-model="vm.title"/>
function SomeController($scope, $log) {
    var vm = this;
    vm.title = 'Some Title';

    $scope.$watch('vm.title', function(current, original) {
        $log.info('vm.title was %s', original);
        $log.info('vm.title is now %s', current);
    });
}
Bindable Members Up Top

[Style Y033]

Place bindable members at the top of the controller, alphabetized, and not spread through the controller code.

Why?: Placing bindable members at the top makes it easy to read and helps you instantly identify which members of the controller can be bound and used in the View.

Why?: Setting anonymous functions in-line can be easy, but when those functions are more than 1 line of code they can reduce the readability. Defining the functions below the bindable members (the functions will be hoisted) moves the implementation details down, keeps the bindable members up top, and makes it easier to read.

/* avoid */
function Sessions() {
    var vm = this;

    vm.gotoSession = function() {
      /* ... */
    };
    vm.refresh = function() {
      /* ... */
    };
    vm.search = function() {
      /* ... */
    };
    vm.sessions = [];
    vm.title = 'Sessions';
/* recommended */
function Sessions() {
    var vm = this;

    vm.gotoSession = gotoSession;
    vm.refresh = refresh;
    vm.search = search;
    vm.sessions = [];
    vm.title = 'Sessions';

    ////////////

    function gotoSession() {
      /* */
    }

    function refresh() {
      /* */
    }

    function search() {
      /* */
    }
Controller Using "Above the Fold"

Note: If the function is a 1 liner consider keeping it right up top, as long as readability is not affected.

/* avoid */
function Sessions(data) {
    var vm = this;

    vm.gotoSession = gotoSession;
    vm.refresh = function() {
        /**
         * lines
         * of
         * code
         * affects
         * readability
         */
    };
    vm.search = search;
    vm.sessions = [];
    vm.title = 'Sessions';
/* recommended */
function Sessions(dataservice) {
    var vm = this;

    vm.gotoSession = gotoSession;
    vm.refresh = dataservice.refresh; // 1 liner is OK
    vm.search = search;
    vm.sessions = [];
    vm.title = 'Sessions';
    
https://github.com/airbnb/javascript
https://github.com/johnpapa/angular-styleguide
