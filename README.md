bolza-modal
=====================

Modal wrapper for [ui.bootstrap.modal](https://github.com/angular-ui/bootstrap/tree/master/src/modal) on AngularJS.
Makes modal configurable inside angular `module.config()`, allows for easy setup, inline open/close actions and passing
data to the modal instance.

To better handle modals in a complex angular application i made a component that wraps the functionality of ui.bootstrap.modal
extending it with more usable methods.
This is composed by a provider and two directives.

## Version Numbers

bolzaModal follows [semantic versioning](http://semver.org/) and uses the following versioning scheme:

 * Versions starting with 0 (e.g. 0.1.0, 0.2.0, etc.) are for initial development, and the API is not stable
 * Versions with an even minor version (1.0.0, 1.4.0, 2.2.0, etc.) are stable releases
 * Versions with an odd minor version (1.1.0, 1.3.0, 2.1.0, etc.) are development releases


## Getting Started

Install with `bower`:

```shell
bower install bolza-modal --save
```

Add a `<script>` to your `index.html`.
Anyway it should be already there if you `grunt serve` the project.

```html
<script src="bower_components/bolza-modal/bolza-modal.js"></script>
```

Add `bolzaModal` as a dependency for your app:

```javascript
angular.module('myApp', ['bolzaModal']);
```

#Usage

##Modal Provider

### .open(name, [data])
opens a modal, returns a promise that will be eventually resolved by the `.close` method

```javascript
modal.open('kitten.create').then(function(myKitten) {
    ...
});
```

name | type | description
--- | --- | ---
name | string | the name of the modal as defined at `add` time, if not defined the last opened modal is used
data | any | eventual data we want to store in the context of this modal instance
returns a promise

### .close([name], [result])
Closes a modal

```javascript
modal.close('kitten.create', {name: 'Kittyt'})
```

name | type | description
--- | --- | ---
name | string | the name of the modal as defined at `add` time, if not defined the last opened modal is used
result | any |data to be passed to the `open` promise callback

### .add(name, config)
Adds the modal configuration object so it is globally known by the application.
Usually used in `angular.module().config` as `modalProvider` to setup the modal alongside the routing.
When using `resolve` object we must use `ngInject` or other techniques to explicitly inject names of services we use in the resolve the function.

```javascript
modalProvider.add('kitten.create', {
    templateUrl: 'app/kitten/create/kittenCreate.html',
    controller: 'KittenCreateController',
    controllerAs: 'vm',
    backdrop: true,
    size: 'full',
    resolve: {
        /* @ngInject */
        kittenList: function(kitten) {
            return kitten.getAll({'per_page':100}).$promise;
        }
    }
});
```

name | type | description
--- | --- | ---
name | string | the name of the modal
config | object | uses the same configuration format as bootstrap-modal and ui-router.state

### .getData([name])
gets the data passed to the of the last modal opened

```javascript
var passedData = modal.getData('kitten.create')
```

name | type | description
--- | --- | ---
name | string | the name of the modal as defined at `add` time, if not defined the last opened modal is used


##Modal Directive

###modalOpen(name)
Will open the modal on click.

```html
<button modal-open="kitten.create">Create a Kitten</button>
```

###modalClose([name])
Will close the modal defined by `name` or the last modal opened if no name is specified.

```html
<button modal-close>Close Me</button>
```
