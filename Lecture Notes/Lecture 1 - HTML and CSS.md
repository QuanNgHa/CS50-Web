# Lecture 1 - HTML and CSS

## Git branching
- Can create a new branch to develop new features 
- Merge branches with master
- HEAD refers to the current branch we're working on
- `git branch` checks what branches there are currently
- `git branch <name of branch>` adds a new branch
- `git checkout <name of branch>` move HEAD to another branch
- `git commit -am <commit message>` shortcut for add and commit

## HTML
### Form
- Adding a name for the input allows us to write code easily to get the name of the form etc.
``` html
<!-- Text field input -->
<form>
    <input name = "name" type = "text" placeholder = "Write your name here">
</form>
```
``` html
<!-- Radio -->
<form>
    <input name = "color" type = "radio" value = "red">Red
    <input name = "color" type = "radio" value = "blue">Blue
</form>
```
## CSS
### Immediate child selector
- `ol > li` will only select all li that are immediate children of ol

### attribute 
- Styling a particular input type 
``` css
input[type=text] {
      background-color: red;
  }
```
### before
- Style the content before a particular element
- In the example below - the words 'click here' before a link will be bold
``` CSS
a::before {
    content: "Click here: ";
    font-weight: bold;
}
```


### selection 
- Style the selection of a particular element 
- In the example below, any paragraphs when selected will be red with a yellow background 
``` CSS
p::selection {
    color: red;
    background-color: yellow;
}
```

### CSS selectors summary

| CSS           | Description        | 
| ------------- |:-------------:     | 
| a, b          | multiple elements  | 
| a b           | descendant         |  
| a > b         | immediate child    |
| a + b         | adjacent sibling   |
| [a=b]         | attribute          |
| a:b           | pseudoclass        |
| a::b          | pseudoelement      |


## Responsive Design

### Media query
- Don't print a particular section 
``` CSS
@media print {
    #print-section {
        display: none;
    }
}
```
- When the windows is at least 800px, the font size is 20px
- When the windows is less than 800px, the font size is 14px
``` CSS
@media (min-width: 800px) {
    body {
        font-size: 20px;
    }
}

@media (max-width: 799px) {
    body {
        font-size: 14px;
    }
}
```

### Flexbox
``` CSS
.container {
    /* FLexbox which allows content to wrap around when it reaches the end of the line*/
    display: flex;  
    flex-wrap: wrap;
}
```

### Grid 
``` CSS
.container {
    display: grid;  
    grid-column-gap: 2%;
    grid-row-gap: 6%;
    grid-template-columns: 200px 200px auto; 
    /* How wide you want each column to be */
    /* 'auto' makes the boxes respond to size of the screen automatically */
}
```

## Sass
- A language built on top of CSS
``` Scss
$color: red; // Define a variable

ul {
    font-size: 14px;
    color: $color;
}
```
- `sass style.scss style.css ` changes a scss file to a CSS file
-  `sass --watch style.scss:style.css` automatically recompiles style.scss as style.css whenever style.scss is modified

### Nesting 
- Style elemets which are inter-related
- In the example below, paragraphs inside a div will be blue with font size of 18px
``` Scss 
div {
    font-size: 18px;

    p {
        color: blue;
    }
}
```

### Inheritance
- Allows for slight tweaking of a general style for different components
``` Scss 
%message { // % defines a template
    font-family: sans-serif;
    font-size: 18px;
}

.success {
    @extend $message; // Take all the styling of the %message template
    background-color: green;
}
```
