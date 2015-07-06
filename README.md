**Guidelines refractoring**

Logique dans les services
Variable avec nom complet ($scope.navigationService = navigationService et pas $scope.ns)
Retirer les console.log et mettre des $log.debug
Insérer $log dans chaque controller, service

les box d’help ou trucs visibles qu’on peut show/hide s’appellent 
vm.isDisplayed.helpMessage (par exemple si c’est un help message), ou bien vm.isDisplayed.helpSelectAMeeting, …
controllerAs VIEW
<!-- recommended -->
<div ng-controller="Customer as customer">
    {{ customer.name }}
</div>
controllerAs in CONTROLLER with vm
/* recommended */
function Customer() {
    var vm = this;
    vm.name = {};
    vm.sendMessage = function() { };
}
Place bindable members at the top of the controller, alphabetized, and not spread through the controller code.
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



https://github.com/airbnb/javascript
https://github.com/johnpapa/angular-styleguide
