# Notes from Udemy Course Advanced CSS and SASS

### Table of Contents
1. BEM syling of CSS
1. SASS Variables


### Resources
Images for project were found at: [Unspash](https://unsplash.com/)


### Process of building CSS sheet
- Block Element Modifier - BEM
    ```css
        .block {};
        .block__element {}; /* two underscores */
        .block__element--modifier {};
    ```

### What is SASS?
- CSS preprocessor, an extension of CSS
- Called preprocessor because a compiler is needed to change it to CSS code
- Two syntax methods, SASS and SCSS, SCSS is used in this course
- [List of resources](https://github.com/HugoGiraudel/awesome-sass) for SASS.
- **Features** ([Link to CodePen](https://codepen.io/zolkocarbon/pen/XEmJJg) using all examples below)
    + Variables: for reusable values such as color and font size
    ```scss
    $color-primary: #f9ed69; //comment in SASS
    ```

    + Nesting: nest selectors inside one another
        ```scss
        .navigation {
            list-style: none;
            float: left;

            li {
                display: inline-block;
                margin-left: 30px;

                &:first-child {
                margin: 0;
                }

                a:link { //translates to .navigation li a:link {}
                @include style-link-text($color-text-dark);
                } 
            }
        }
        ```
    + Operators: for mathmatical operations inside CSS (example below in [Functions](#functions))
    + Partials and imports: to write CSS in different files and import them all into one single file
    + Mixins: to write reusable pieces of CSS code
        ```scss
            @mixin clearfix {
                &::after {
                    content: "";
                    clear: both;
                    display: table;
                }
            }

            nav {  
                @include clearfix
            }
        ```
        A mixin with an argument
        ```scss
            @mixin style-link-text($color) {
                text-decoration: none;
                text-transform: uppercase;
                color: $color;
            }

            .btn-hot:link {
                @include style-link-text($color-text-light);
            }
        ```
    + <a name="functions"> Functions: like mixins, with the difference that they produce a value for later use </a>
        ```scss
        @function divide($a, $b) {
            @return $a / $b;
        }

        nav {
            margin: divide(60, 2) * 1px;
        }
        ```
        SASS also features built-in functions. [Link to list](http://sass-lang.com/documentation/Sass/Script/Functions.html) of built in fuctions.
    + Extends: to make different selectors inherit declarations that are common to all of them
        Before using extends
        ```scss
        .btn-main:link,
        .btn-hot:link {
            padding: 10px;
            display: inline-block;
            text-align: center;
            border-radius: 100px;
            width: $width-button;
            @include style-link-text($color-text-light);
        }

        .btn-main {
          &:link {
            background-color: $color-secondary;
          }

          &:hover {
            background-color: darken($color-secondary, 15%);
          } // darken is built-in SASS function
        }
        ```
        After using extend
        ```scss
            %btn-placeholder {
                padding: 10px;
                display: inline-block;
                text-align: center;
                border-radius: 100px;
                width: $width-button;
                @include style-link-text($color-text-light);
            }

            .btn-main {
                &:link {
                  @extend %btn-placeholder;
                  background-color: $color-secondary;
                }

                &:hover {
                    background-color: darken($color-secondary, 15%);
                } // darken is built-in SASS function
            }
        ```
        Note: while this may look similar to a mixin the complied CSS code does look different.
        The extend will create a class selector .btn-main:link, .btn-hot:link {} while the mixin would 
        create duplicates of code in the btn-main and btn-hot seperate selectors.
    + Control directives: for writing complex conditionals and loops (not covered in this course)

## Nodejs installation and SASS setup
1. Install nodejs from [here](https://nodejs.org/en/) on your computer.
    > Note: I was having issues installing node. After an hour of searching forums it turned out that I needed to turn off the virus protection.

1. Confirm node is installed by typing *node -v* in the terminal. (I did this in the Visual Studio terminal)

1. Navigate into project folder using the terminal commands.

1. Create package.json file by typing *npm init* in the terminal.
    + The prompt will prompt several questions for creating the file. In most cases the fields can be left blank or default values used.
    + If you download/clone someone's project and want to install all dependencies simply run *npm install* in the terminal and 
    all dependencies in the package.json file will be installed. This illustrates the main purpose of this file. Another scenario is working on one project on more than one computer.
    + When uploading a project to a repository don't upload the node_modules file as it is not necessary.

1. Install SASS npm package by typing *npm install node-sass --save-dev* in the terminal.
    + The --save-dev saves the development dependencie in the package.json file.
    + Other packages such as jQuery should be saved as *--save* because they are non-development dependencies.
    + To uninstall a package (jQuery example) type *npm uninstall jquery --save* in the terminal.

## Creating and compiling SASS/SCSS file
1. Create sass folder.
1. Create main.scss file.
1. Copy all content of style.css into it.
1. Modify package.json file
    ```javascript
        "scripts": {
            "compile:sass": "node-sass sass/main.scss css/style2.css -w"
        },
    ```
    + *compile:sass* is the name of the command we will use in the terminal.
    + *sass/main.scss* is the name and location of the SASS file to compile.
    + *css/style2.css* is the CSS file you want SASS to create and it's location.
    + *-w* is a watch command that will keep the compiler running in the terminal and update the CSS file every time the SASS file is saved.
1. Type *npm run compile:sass* in the terminal to compile the SASS file.

## Automatically reloading a page on file changes
1. Run *npm install live-server -g* to install live-server globally
    + The *-g* is for a global installation of the package so that it could be used on any project.
1. Run *live-server* to activate the package which will automatically open the html file.
    + This will run a local server and refresh the page every time a page edit and save occurs.
    + For this to work with the SASS compiler we need two terminals running, one for the SASS compiler watching and one for this package.

## Natours project SASS Nesting
Note: original CSS file is style.css and it has a lot of annotation to the style. main.scss is now the new style sheet.
```scss
    .header {
        //some properties here for header selector
        &__logo-box {
            //this is equivalent to .header__logo-box
        }
    }
```
Example before applying SASS nesting
```scss
.header {
    height: 95vh;
    background-image: linear-gradient(
        to right bottom,
        rgba(126, 213, 111, 0.80),
        rgba(40, 180, 133, 0.80)),
        url(../img/hero.jpg);
    background-size: cover;
    background-position: top;
    clip-path: polygon(0 0, 100% 0, 100% 75vh, 0 100%);
    position: relative;
}

.header__logo-box {
    position: absolute;
    top: 4rem;
    left: 4rem;
}

.header__logo {
    height: 3.5rem;
    backface-visibility: hidden;
}
```
After nesting and SASS variables change
```scss
.header {
    height: 95vh;
    background-image: linear-gradient(
        to right bottom,
        rgba($color-primary, 0.80),
        rgba($color-primary-dark, 0.80)),
        url(../img/hero.jpg);
    background-size: cover;
    background-position: top;
    clip-path: polygon(0 0, 100% 0, 100% 75vh, 0 100%);
    position: relative;

    &__logo-box {
        position: absolute;
        top: 4rem;
        left: 4rem;
    }

    &__logo {
        height: 3.5rem;
        backface-visibility: hidden;
    }
}
```

## Natours Project 7-1 Architecture
1. [Link to 7-1 Architecture](https://github.com/HugoGiraudel/sass-boilerplate) example on GitHub as a Boilerplate.
1. Create *base* folder in sass folder.
1. Create *_base.scss* file inside base folder. All partial files start with an underscore for convention.
1. Add the following to the top of main.scss. This will include the partial. Note that the underscore and .scss are not required as shown here.
    ```scss
    @import "base/base";
    ```
1. Create remaining files in SASS folder which can be seen in this repo or the link above.

## Layout types
    + Float Layout: most supported in browsers and used in this project.
    + Flexbox
    + CSS Grid

## Implementing Float Layout
```scss
.row {
    max-width: $grid-width; //take up 1140px if available, 1140px is standard
    background-color: #eee;
    margin: 0 auto; //center row
    
    &:not(:last-child) { // all rows except the last one
        margin-bottom: $gutter-vertical;
    }

    .col-1-of-2 {
        width: calc((100% - #{$gutter-horizontal}) / 2);
        background-color: orangered;
        float: left;
        
        &:not(:last-child) { //margin for all columns except the last one (furthest right)
            margin-right: $gutter-horizontal;
        }
    }
}
```
Since the only property that will change between columns is the width we can move the rest of the styles to seperate selector
```scss
.row {
    max-width: $grid-width; //take up 1140px if available, 1140px is standard
    background-color: #eee;
    margin: 0 auto; //center row
    
    &:not(:last-child) { // all rows except the last one
        margin-bottom: $gutter-vertical;
    }

    @include clearfix;

    [class^="col-"] { //select all classes that start with "col-"
        background-color: orangered;
        float: left;
        
        &:not(:last-child) { //margin for all columns except the last one (furthest right)
            margin-right: $gutter-horizontal;
        }
    }

    .col-1-of-2 {
        width: calc((100% - #{$gutter-horizontal}) / 2);
    }
```
## Emmet in Visual Studio Code
+ This is an extension built-in to VSC that allows fast html writing.
+ [Instructors cheat sheet.](https://docs.emmet.io/cheat-sheet/)
+ To write line below type *.text* and then press TAB.
```html 
<div class="text"></div>
```
+ To write line below type *section.text* and then press TAB.
``` html 
<section class="text"></section>
```
+ *.composition>(img.composition__photo.composition__photo--p1)\*3* produces
    ```html
    <div class="composition">
        <img src="" alt="" class="composition__photo composition__photo--p1">
        <img src="" alt="" class="composition__photo composition__photo--p1">
        <img src="" alt="" class="composition__photo composition__photo--p1">
    </div>     
    ```


## Next section of project <main>
+ Thinking about components: the section after the header has components such as buttons, typography and images.
+ How and why to use utility classes: to use a common style throught website
```scss
.u-center-text { text-align: center; }
```
+ How to use the *background-clip* property. This will allow the background to only show behind the text
```scss
background-image: linear-gradient(to right, $color-primary-light, $color-primary-dark);
-webkit-background-clip: text; // background gets clipped exactly where text is
color: transparent;
```
+ How to *transform* multiple properties simultaneously
```scss
transform: skewY(2deg) skewX(15deg) scale(1.1);
```
+ How to use the *outline-offset* property together with *outline* to create an outline around an image with an offset. Example can be found in [this file](sass/components/_composition.scss)
```scss
.composition {

    &__photo {
        outline-offset: 2rem;

    &:hover {
        outline: 1.5rem solid $color-primary;
}
```
+ How to style elements tha are NOT hovered while others are. Example can be found in [this file](sass/components/_composition.scss)
```scss
.composition {
    position: relative;
    transition: all 0.2s;

    &:hover &__photo:not(:hover) { 
        transform: scale(0.95);
    }

    &__photo {

        &:hover {
            transform: scale(1.05);
        }
    }
}
```

## Building the features section
#### How to include and use the icon font.
+ [Link to Linea](http://linea.io/) which is the icon font used in this course. Download all sets.
+ In this project we used the *basic/_ICONFONT/fonts* and *basic/_ICONFONTS/styles.css* files and moved them into the project css folder along with the [icon reference file](css/fonts/icons-reference.html)
+ While all sets were downloaded the features section uses fonts from all the options (SVG, PNG, Fonts) in order to get the gradient background effect for the icon.
+ Note: renamed styles.css to icon-font.css
+ Syle sheet was added to the index page
```html
<link rel="stylesheet" href="css/icon-font.css">
```

+ Use the icon reference file to identify which icon you want to use and add *icon-* in front of it as a class name

```html
<i class="feature-box__icon icon-basic-map"></i>
```
#### Another way to create the "skewed section" design.
+  [Used here](sass/pages/_home.scss) for this project.
+ This will skew all the content. The background image and the four boxes in the section. To undo the skew for the boxes look at the next section.
```scss
.section-features {
    transform: skewY(-7deg);
}
```
#### How and when to use the direct child.
+  Also [used here](sass/pages/_home.scss) in this project. Here we undo the skew for the four boxes in our feature section.
```scss
.section-features {
    transform: skewY(-7deg) translateY(-10rem);

    & > * { // select direct child, which in this case is .row
        transform: skewY(7deg);
    }
}
```
HTML for this section where direct child selected is row
```html
<section class="section-features">
    <div class="row">
    </div>
</section>
```

## Building the tours section
This section of the project has three rotating cards, covers perspective, backface-visibility, background blend models and box-decoration-break. Card properties are defined in [this file](sass/components/_card.scss)

#### How to use perspective in CSS
This property skews the item in a transform to give it a feeling of getting closer to the page. Like flipping page in a book.
This example has will rotate the card 180 degrees in the Y axis and skew the card during the rotation.

The lower the number in the *perspective* property the higher the skew effect.
```scss
.card {
    perspective: 150rem; // choose huge number, arbitrary
    -moz-perspective: 150rem; // firefox coverage

       &:hover &__side--front { // .card:hover .card_side--front
        transform: rotateY(180deg);
    }

    &:hover &__side--back {
        transform: rotateY(0);
    }    
}
```

#### How to use the backface-visibility property
In order to hide the back of the card we use this property
```scss
.card {

    &__side {
        backface-visibility: hidden;
```
#### Using background blend modes
This property blends layers, in our project its a gradient and an image. 

Using this property causes the image to overflow outside parent so the fix is to use `overflow: hidden`

Not supported in Internet Explorer.

[W3 Documentation for different modes](https://www.w3schools.com/cssref/pr_background-blend-mode.asp)
```scss
    // FRONT SIDE STYLING
.card {

   &__side {
       
        border-radius: 3px;
        overflow: hidden; //hides image overflow from image property "background-blend-mode" below

    &__picture{ //in child div of above selector
        background-blend-mode: screen;

        &--1 {
            background-image: linear-gradient(to bottom right, $color-secondary-light, $color-secondary-dark), 
            url(../img/nat-5.jpg)
        }
```

#### How and when to use `box-decoration-break`
<img src="img/boxdecoration_before.jpg" width="400"><img src="img/boxdecoration_after.jpg" width="400">

The `box-decoration-break` property is used to here for the text that overflows to the next line, "THE SEA EXPLORER".
On the left is the original element without the style applied. Since it overflows there is no padding added between the SEA and 
EXPLORER words as it is being treated as a single element. The right is after the property where the padding is applied evenly around the 
entire text.
```scss
.card {
    &__heading-span {
        padding: 1rem 1.5rem;
        -webkit-box-decoration-break: clone;
        box-decoration-break: clone;
```
```html 
<span class="card__heading-span card__heading-span--1">
        The Sea Explorer
</span>
```
## Building the customer feedback section

#### How to make text flow around shapes with `shape-outside` and `float`

#### How to apply a `filter` to images

#### How to create a background video covering an entire section

#### How to use the `video` HTML element

#### How and when to use the `object-fit` propert





    





