Introduction.

The main function for creating and running variant of test is variant.run
This function can apply 4 parameters:
test.variant([html], [css], [js], [options])
Each parameter of function is optional and no required the order. 
* Running function without arguments will be generated variant of test as default.

Parameters:

*** html ***
Parameter html provides easiest way for creating template.
* Calling function only with this parameter will be append template into body. 
 
Example:
```javascript
variant.run(
    '<h1> Test is run. <h1>'
);
```

*** css ***
Parameter css add style to the test page.

Example:
```javascript
variant.run(
    `body {
        background: rgb(99, 99, 206);
        color: white;
    }`
);
```

Example with template:
```javascript
variant.run(
    '<h1> Test is run. <h1>',
    `h1 {
        background: palegreen;
        color: green;
    }
    body {
        background: rgb(99, 99, 206);
        color: white;
    }`
);
```

*** js ***
Parameter js provides the function for execution javascript.
It should be a function which will be executed when test is uploaded.
* Context of this function contains other parameters which available into context as a functions.

Example:
```javascript
test.variant(
    function () {
        alert('run test');
    }
);
```

Context of template is available by calling this.html function:

Example with passing template into specific place:
```javascript
variant.run(
    '<h1> Test is run. <h1>',
    function () {
        document.getElementById('header').innerHTML(this.html());
    }
);
```
 
Defined data for template is easy by creating a new property into context of execution function. 
Data into template will be available as a name of property into curly brackets. 
* For using brackets as text it should be encoded by slash /{ /}
* Do not use next names into properties: html, css, options they will be ignored

Example:
```javascript
variant.run(
    '<h1> Test is run. {text} <h1>',
    function () {
        this.text = "Is amazing!!!"
        document.getElementById('header').innerHTML(this.html());
    }
);
```

*** options ***
Parameter options provides cleaner way for split data from execution function.
Instead of defined data into execution function put it into options object.

Example:
```javascript
variant.run(
    '<h1> Test is run. {text} <h1>',
    {
        text: "Is amazing!!!"
    }
);
```

Data for template can be also an array which will be duplicate a wrapper element
 
Example:
```javascript
variant.run(
    `<div>
        <h1> Test is run. {text} <h1>
        <ul>
            <li>{items}</li>
        </ul>
     </div>`,
    {
        text: "Is amazing!!!",
        items: ['']
    }
);
```

Example with all parameters:
```javascript
variant.run(
    '<h1> Test is run. {text} <h1>',
    'h1 {
        background: palegreen;
        color: green;
    }',
    function () {
            document.getElementById('header').innerHTML(this.html());
    },
    {
        text: "Is amazing!!!"
    }
);
```