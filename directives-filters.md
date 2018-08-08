---

class: middle 
.left-column[
## Directives and Filters
]

.right-column[
In addition to the core directives such as ```v-model``` and ```v-for``` Vue allows you to create custom directives.  Though Components
are the preferred method to achieve maximum re-usability you might find yourself in a situation that is better suited to a custom directive.
<br/>
<br/>
**Filters** are most commonly used for text formatting but can be used for a variety of data transformations.
]


---

class: middle 
.left-column[
## Directives and Filters

### Custom Directives
]
.right-column[

A simple custom directive looks like this:
```xml
<span v-pin>This content is pinned</span>
```
We add the directive to our html element.

```javascript
export default function (element) {
    element.style.bottom = '10px';
    element.style.right = '5px';
    element.style.position = 'absolute';
}
```
We export a javascript object implementing the directive

```javascript
import pinDirective from './shared/pin-directive';

export default {
    name: 'my-component'
    directives: { pin },
    ...
};
```
Finally we register the directive with our component.
]

---

class: middle 
.left-column[
## Directives and Filters

### Custom Directives
### Passing Data to Directives
]

.right-column[
The easisest way to pass paramters to a directive is with object notation:
```xml
<span v-pin="{bottom: '10px', right: '5px'}">This content is pinned</span>
```

```javascript
export default function (element, binding) {
    Object.keys(binding.value).forEach((position) => {
        element.style[position] = binding.value[position];
    });

    element.style.position = 'absolute';
}
```
]


---

class: top 
.left-column[
## Directives and Filters

### Custom Directives
### Passing Data to Directives
### Directive Lifecyle Hooks
]

.right-column[
    Like components directives and filters have lifecycle hooks that you can tie into:

```javascript
export default function (element, binding) {
    bind: (element, binding) => {
        doSomething(element, binding);
    },
    update: (element, binding) => {
        doSomething(element, binding);
    }
}
```
]

---

class: top 
.left-column[
## Directives and Filters

### Custom Directives
### Passing Data to Directives
### Directive Lifecyle Hooks
### Custom Filters
]

.right-column[
Filters are typically used to format data for display.  A good example is currency:
```xml
<td class="cost">{{robot.cost | currency }}</td>
```
We invoke a filter with the pipe operator:

```javascript
export default function (amount) {
    return `$${amount.toFixed(2)}`;
}
```
]

---

class: top 
.left-column[
## Directives and Filters

### Custom Directives
### Passing Data to Directives
### Directive Lifecyle Hooks
### Custom Filters
]

.right-column[
You can also pass parameters into filters:
```xml
<td class="cost">{{robot.cost | currency('$') }}</td>
```
We invoke a filter user the pipe operator

```javascript
export default function (amount, symbol) {
    return `${symbol}${amount.toFixed(2)}`;
}
```
]

---

class: top 
.left-column[
## Directives and Filters

### Custom Directives
### Passing Data to Directives
### Directive Lifecyle Hooks
### Custom Filters
### Global Accessibility
]

.right-column[
To make our directives or filters available globally we register them with the application bootstrapper.
```javascript
import pinDirective from './shared/pin-directive';
import currencyFilter from './shared/currency-filter';

Vue.directive('pin', pinDirective);
Vue.filter('currency', currencyFilter);

new Vue({
    render: h => h(App),
    router,
    store
}).$mount('#app');
```
]