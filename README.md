# VARIANTS
## variant.set([ html ], [ css ], [ js ], [ options ])
- The main function for creating and passing content to the page is `variant.set`
- Function `variant.set` can apply **4 optional parameters** in any order
- Running function without arguments will be generate variant with default content

## Parameters:

### *[ html ]*
- `html` parameter provides easiest way for creating and passing template to the page.
- Invoke function only with `html` parameter will be append template into body. 

*Example of appending template into tag body:*
```javascript
variant.set('<h1> Test is run. <h1>');
```

### *[ css ]*
- `css` parameter is style. Style apply to the page after variants generation

*Example of adding new style to the page:*
```javascript
variant.set(
    `body {
        background: rgb(99, 99, 206);
        color: white;
    }`
);
```

*Example of adding style and template to the page:*
```javascript
variant.set(
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

### *[ js ]*
- `js` parameter is a function. Code of `js` function would be execute after variants generation

*Example of running js when variant was generated:*
```javascript
variant.set(
    function () {
        alert('variant is generated');
    }
);
```

- Context of `js` function contains content of `css` and `html` parameters as a **functions** 
which available by same names `this.css()` and `this.html()`

*Example of passing template into tag header:*
```javascript
variant.set(
    '<h1> Run test. <h1>',
    
    function () {
        document.getElementById('header').appendChild(this.html());
    }
);
```

- Dynamic data into template which defined as **property** into context of `js` parameter 
is available by the same **name into curly brackets**. 
- For using brackets as a simple text it should be encoded by slash for the left `/{` and for the right `/}`
- Reserves names of property: `html`, `css`

*Example of passing data into template:*
```javascript
variant.set(
    '<h1> Run test. {text} <h1>',
    
    function () {
        this.text = 'It's easy!!!'
        
        document.getElementById('header').innerHTML(this.html())
    }
);
```

- Data for template can be one of the next type: **string** or **array** 
- In case of data is **array** and it is **not empty** wrapper tag will be duplicates for each element
- In case of data is **array** and it is **empty** wrapper tag will be removed

*Example of passing different type of data into template:*
```javascript
variant.set(
    `<section>
        <h1> Run test. {text} <h1>
        <ol>
            <li> {parameters} </li>
        </ol>
    </section>`,
    
    function () {
        this.text = `It's easy!!!`
        this.parameters = ['html', 'css', 'js', 'options']
        
        document.getElementById('header').innerHTML(this.html())
    }
);
```
### *[ options ]*
- `options` parameter is object which provides cleaner way for splitting data from execution function

*Example of putting data into options:*
```javascript
variant.set(
    `<section>
        <h1> Run test. {text} <h1>
        <ol>
            <li> {parameters} </li>
        </ol>
    </section>`,
    
    function () {
        document.getElementById('header').innerHTML(this.html())
    },
    
    {
        text: `It's amazing!!!`,
        parameters: ['html', 'css', 'js', 'options']
    }
);
```

## variant.conf( settings )
- For keeping control on generation process invoke function `variant.conf` with specific settings

## settings:
- `settings` is a object with specific keys which help to keep control on generation process

### *settings.checker { Function }*
- Key `checker` is a function
- Function `checker` will executed in interval
- When function `checker` returns **true** variant starts generation
- By default `checker` returns **true** on **DOMContentLoaded** event

### *settings.interval { Number }*
- Key `interval` is a time of duration when `checker` function is invoke
- Values of `interval` sets in *ms*
- Default value of `interval` *100*

### *settings.stop { Function }*
- Key `stop` is a function
- When `stop` function returns **true** `checker` will be stopped.

### *settings.stopInterval { Number }*
- Key `stopInterval` is a time after DOMContentLoaded when `checker` will be stopped.
- Values of `stopInterval` sets in *ms*

### *settings.order { Number }*
- Key `order` sets an order of execution variant for multi variant case  
- Default values of `order` is *0*

### *settings.hide { String }*
- Key `settings` is selector which hide during `checker` process

### *settings.hideStyle { String }*
- Key `hideStyle` is a rule for hidden element
- Default values of `hideStyle` is `position: absolute; top: -10000px; left: -10000px;`