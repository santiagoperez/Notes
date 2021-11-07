## 5 Conditional content

Module content

1. Outputting dynamic lists of content

1. Rendering content under certain conditions

## 5.1 Render dynamically

In order to render dynamically array and list of objects, we can leverage built-in JS functions such as map. As explained in previous lessons, map acts on an array by executing a function in every single element on the array.

Because of this, we can invoke on each one of the elements JSX components rendering which will evaluate and result on an array of elements and react will be able to modify them dynamically as long as we act over the array. By leveraging useState we will be able to modify the array adding or deleting elements. Array can also be filtered based on specific conditions.

## 5.2 Conditional rendering

JSX also allows us to add conditional statements with ternary conditions. E.g. to show a warning

```js
{filteredExpenses.length === 0 ? (<p>Error</p>) : {return ...}}
```

As the regular expressions get larger, it is a good practice to leave JSX code in the function cleaner and evaluate that business logic outside of it while still in the component.

We can also dynamically change styling by modifying css properties inside oru HTML components

```js
let barFillHeight = '0%';

if (props.maxValue > 0) {
    barFillHeight = Math.round((props.value / props.maxValue) * 100) + '%';
}

<div className="chart-bar__fill" style={{height: barFillHeight}}></div>
```