## 6 Styling components

Module content

1. Conditional & Dynamic Styles

1. Styled components

1. CSS Modules

### 6.1 Setting dynamic inline styles

Sometimes we want to add a style dynamically, for example if the user input is incorrect, we want to show a warning/error message. We achieve that by looking at the component state and reacting based on that. The JSX component declaration can be modified later based on that state and the ```style``` property with regular JS code.

```js
<label style={{color: !isValid ? 'red' : 'black'}}>Course Goal</label>
```

However, inline style override default styles we have defined in our CSS, so we have to duplicate some code.

### 6.2 Setting dynamic CSS classes

In order to avoid this duplication of code we can somehow define different CSS classes applying dynamically on how the component is reacting to the input. For example, for error input we can import a CSS class and for success another.

In order to do that we don't leverage inline style but we change the div className directly, and form the class string to be imported through literals. (symbol **`**).

```js
<div className={`form-control ${!isValid ? 'invalid' : ''}`}> //appends invalid if the state is set to isValid=false

//CSS file will contain a .form-control class and a .form-control.invalid class
```

### 6.3 Styled components

Styled components is a library which allows building components with styles affecting only to that scope. We don't have to import css files but they will be set up in code; which avoids in specific cases collision with other parts of our project.

```styled-components``` is a third party library which has to be installed into our project. The way we use it is by creating an *template literal* where we can embed expressions. We call a method inside the ```styled``` object and literal it so that it passes through the method. Inside the literal we add our style directly (without defining classes). Pseudo elements in the style will be referenced through ```&```.

```js
import styled from 'styled-components';

const Button = styled.button`
color: black ;
(...)

&:focus { // pseudoclass, equal to .button:focus
    outline: none;
}
`;
```

This doesn't apply only to a whole components but can be valid to elements inside that component.

We can append to a component rendered through styled-components further styles (through className). But we can also logically add it in the style definition, by passing the data (as if it was a prop) and use JS logic through ```${}```.

```js
const FormControl = styled.div`
    color: ${props => props.invalid ? 'red' : 'black'};
`;

<FormControl invalid={!isValid} />
```

Styled-components have a concept called media queries where, when the browser sets specific circumstances (e.g. big or small width in desktop vs mobile), change specific styles. This is useful for responsive UIs on all type of devices.

```js
const Button = styled.button`
width: 100%;

@media (min-width: 768px) {
    width: auto;
}
`;
```

### 6.4 CSS Modules

An alternative to *styled-components* is to use *CSS Modules*. This allow programmatically define the style through brackets creating unique an unique class for each component, so that we can modify or adapt depending on our needs.

In order to use CSS Modules we need to import our CSS differently ```import styles from 'CSS-file'``` and rename the CSS file to ending in ```.module.css```. Every class defined in the CSS will have a method inside the styles object.

```js
import styles from './Button.module.css'; //CSS contains a .button class

<button className={styles.button}>
```

For composed name classes (e.g. ```form-control``` instead of ```button```) we can access through ```styles['form-control']```. String literals also can apply here to dynamically change the class based on an input 

```js
<div className={`${styles['form-control']} ${!isValid && styles.invalid}`}>
```

Media queries are applied directly on the CSS class, no need to do further changes in the JSX.