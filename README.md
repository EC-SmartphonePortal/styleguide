# Front-end Coding Standards #

## Overview ##

The objective of these guidelines is to:

- Be able to work with multiple web designers on the same codebase in the same way
- Write code that is less prone to bugs and regressions, is easier to read, maintain and debug
- Write code that supports re-use

## Contributing ##

The priority in this case remains **consistency** more than **web designers freedom**. However, this document will evolve to reflect the needs of the work we’ll have to do. Here’s how you can contribute to it:

- In the repo, create a *proposal/new-guideline branch* (the name needs to reflect the addition)
- Otherwise fork the repo and when ready create a pull request

## General guidelines ##

#### Language ####

For every project that needs to be deployed in multiple countries, **English** only will be used for class names, etc.

Avoid naming classes with names in local language. It makes things complicated for other countries.

#### Performance ####

Page load times (both real and perceived) are an important thing to consider for users of all browsers and device types. There are some general things we can do it:

- Avoid unnecessary use of `display: none;`
- Keep CSS selectors concise and avoid slow selectors

#### Don't repeat yourself (DRY) ####

If you repeat anything that has already been defined in code, refactor it so that it only ever has one representation in the code.

Respecting this will ensure that you will only ever need to change one implementation of a feature without worrying about needing to change any other part of the code.

#### Separation of concerns ####

Separate *structure* from *presentation* from *behavior* to aid maintainability and understanding.

- Avoid writing inline CSS or JavaScript in HTML
- Avoid writing CSS or HTML in JavaScript

#### Write code to be read ####

Write code that is easy to read for other front-end devs. Don’t be too clever, write it in an easy and understandable way for others.

#### Commenting ####

Don’t leave pieces of code that are commented out in the codebase. It makes the code look unfinished and we have SVN logs to keep a history of all the pieces of code that were written.

Re-use the comment formatting already in place to keep hierarchy consistent across all files.

Use code comment annotations to mark parts of your code that require further work. `@todo` is recognized by Mixture and Visual Studio and can easily be listed for better measurement and management.

- `@todo`: Task to be completed
- `@optimise`: something working but that could be refactored (not recognized)

## CSS & SASS ##

#### General guidelines ####

Our approach to CSS is influenced by Nicole Sullivan's [OOCSS](http://oocss.org/) ideas, and Jonathan Snook's Scalable and Modular Architecture for CSS ([SMACSS](http://smacss.com/)), both of which advocate a general separation of concerns to promote re-usability and prevent code bloat.

- Promote scalable and modular CSS architecture using the principles defined in the SMACSS style guide
- Use the [BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) naming convention (‘Block’, ‘Element’, ‘Modifier’ methodology).
- Use classes rather than element selectors to de-couple CSS from HTML semantics and ensure  that your code doesn’t impact how it may be integrated into backend systems
- Make layouts fluid, using variable units of measurement
- Avoid using id selectors as much as possible as they may need to be re-written during system integration
- Don’t specify units for zero values (`margin: 0;` instead of `margin: 0px;`)
- Use 0 instead of none (`border: 0;` instead of `border: none;`)
- Use shorthand properties (`margin: 1px;` instead of `margin: 1px 1px 1px 1px;`)

#### Depth of applicability ####

Don’t over-specify CSS selectors because they lead to subsequent selectors needing to be of an even higher specificity. (See SMACSS:  [depth of applicability](http://smacss.com/book/applicability)).

    // NO
    #sidebar div ul > li {
      margin-bottom: 5px;
    }

The above example is tightly coupled to the HTML structure which will prevent re-use. Also, if the HTML needs to be changed, the styling will break.

#### Over qualification ####

Don’t qualify ids or classes with tag names.

    // Over qualified id selector
    ul#main-navigation {
      ..
    }
    
    // Over qualified class selector
    table.results {
      ..
    }

In the above example, site structure will be bound with presentation making the site harder to maintain and preventing re-use.

#### High performance CSS ####

Understand that selectors have varying levels of efficiency. Be aware of slow selectors.

Examples :

- The universal attribute `*`
- Attribute selectors `[class=”ico-button”]`
- Regular expression attribute selectors `[class^=”ico-“]`

CSS3's pseudo selectors (`first-child`, …) allow us to make markup cleaner, but the payoff is performance. Keep in mind that promoting HTML semantics at the cost of browser rendering times should remain balanced.

#### Coding style ####

- Always use tabulation (tab key) when indenting code, never spaces.
- Use multi-line CSS. Separate selectors and declarations by new lines. *(It facilitates merging and conflict management in SVN)*.
- Put spaces after the `:` in property declarations.
- Put spaces before `{` in property declarations.
- Use hex color codes `#000` unless using rgba.
- With SASS, use `//` for comments about code . *(Those comments won’t be appear in the CSS output)*.

###### Example of syntax: ######

    // ##################################################
    // ### [ FEATURE TITLE IN CAPS ]
    // #
    .my-block {
      width: 100%;
      color: #000;
    }
    // This is a sub-comment
    .my-block__my-element--first {
      color: #ccc;
      border: 1px solid #000;
    }

#### Margins and consistency ####

As much as possible, we use bottom margins to separate blocks on an HTML page (avoid using top margins except if necessary). Also, to keep the spaces between blocks consistent through the whole website, define 3 to 5 margins as variables (SASS) and stick to it (Some exceptions are possible but they should remain rare)

###### Good example of predefined margins: ######

    // Predefined margins for the whole website
    $margin--very-small: 10px;
    $margin--small: 15px;
    $margin--medium: 20px;
    $margin--large: 35px;
    $strong: 700;
    
    // Code that uses these margins
    .myclass {
      margin-bottom: $margin--small;
    }

###### Wrong use of bottom margins: ######

    // Custom margins added everywhere
    .my-title {
      margin-bottom: 12px;
    }
    .my-subtitle {
      margin-bottom: 7px;
    }
    .my-text {
      margin-bottom: 8px;
    }

#### Reusable elements ####

When you develop new things, think in terms of a library of reusable elements. It helps having a more consistent website which is easier to maintain.

A good example of a reusable element is the [Media element](http://www.stubbornella.org/content/2010/06/25/the-media-object-saves-hundreds-of-lines-of-code/). This article explains how, starting from one generic element, you can use it in a lot of combinations and save a lot of coding.


#### REM ####

Use **rem** (relative em) rather than em or px.

Em allow users to resize type in their browser, so are better for accessibility. But they are complicated to use because they refer to the size of their parent. **Rem units are relative to the HTML element**, which makes them a lot easier to use.

**Use the rem mixin provided** that will automatically prefix the rem value with a pixel value so that older versions of IE will be supported.

## SASS ##

#### Nesting ####

We limit the use of nesting to its minimal. The reason is that with multiple front-end devs in different countries using it (or not), it can do more damage than benefit regarding readability of code and increase of specificity.

**Use nesting only for hover/focus state** and some special cases (see below)

###### Right use of nesting: ######

    // Use nesting for hover, focus, ... states
    // And in some special cases
    .my-class .my-link {
      color: #000;
      &:hover {
        color: #ccc;
      }
      .lt-ie9 & {
        top: 0;
      }
    }

###### Don't use nesting for: ######

    .my-class {
      .my-link {
        color: #000;
        &:hover {
          color: #ccc;
        }
        .lt-ie9 & {
          top: 0;
        }
      }
      .my-subclass {
        ...
      }
    }

#### Usage of variables ####

Because of localization concerns, CSS properties that can change from one country to another will be externalized into a variable file.

    variables-es.scss
    variables-fr.scss

Into these files, a list of variables will be created. The value of those variables can be set directly into the file or linked to a global variable that was defined as “base” (for example, the main color used on the website)

    // ##################################################
    // ### [ BASE ]
    // #
    $primary__color: #f8a01e;
    $secondary__color: #1e5ba3;
    
    $global__white: #FFFFFF;
    $global__grey: #303030;
    $global__middle-grey: #979797;
    $global__beige: #f9f8f7;
    $global__black: #000000;

    // ##################################################
    // ### [ BUTTONS ]
    // #
    $primary-btn__text__color: $global__white; // Variable is linked to a "base" variable
    $primary-btn__bg__top__color: #4779b4;
    $primary-btn__bg__bottom__color: #1f5ca3;
    $primary-btn__bg__color--hover: #184275; // Value is directly assigned to the variable

#### Extends ####

Use them wisely to keep the code clean. Preferably combined with silent classes (see the [SASS documentation](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#placeholder_selectors_) for more info)
Remember that extends are more efficient than includes once compiled as includes will copy all the declarations of the included class to the new class you’ve just created.

###### Example of extends: ######

    %my-generic-class {
      margin: 5px;
      padding: 5px;
      border: 1px solid #000;
      font-size: 12px;
    }
    
    .my-class--1 {
      @extend %my-generic-class;
    }
    .my-class--2 {
      @extend %my-generic-class;
    }
    .my-class--3 {
      @extend %my-generic-class;
    }

    // Ouput (More optimised code)
    .my-class--1,
    .my-class--2,
    .my-class--3 {
      margin: 5px;
      padding: 5px;
      border: 1px solid #000;
      font-size: 12px;
    }

###### Example of includes: ######

    .my-generic-class {
      margin: 5px;
      padding: 5px;
      border: 1px solid #000;
      font-size: 12px;
    }
    
    .my-class--1 {
      @include my-generic-class;
    }
    .my-class--2 {
      @include  my-generic-class;
    }
    .my-class--3 {
      @include  my-generic-class;
    }

    // Ouput (A lot more code)
    .my-class--1 {
      margin: 5px;
      padding: 5px;
      border: 1px solid #000;
      font-size: 12px;
    }
    .my-class--2 {
      margin: 5px;
      padding: 5px;
      border: 1px solid #000;
      font-size: 12px;
    }
    .my-class--3 {
      margin: 5px;
      padding: 5px;
      border: 1px solid #000;
      font-size: 12px;
    }

#### Placeholders selectors ####

Also called “Silent class”. A good article about the subject can be found [here](http://blog.teamtreehouse.com/extending-placeholder-selectors-with-sass).

#### Mixins ####

A list of mixins was defined for the Smartphone Portal. Some explanation and possible values are available in the comments of the `mixins.scss` file. Currently, there is a mixin available for:

- The box model (not compatible IE8!)
- Management of underlining links
- Background gradient
- Border-radius
- Box shadow
- REM mixin + fallback in pixels

###### Example of mixin and how to use it: ######

    // ##################################################
    // ### [ BORDER RADIUS ]
    // #
    @mixin border-radius($radius){
    	-webkit-border-radius: $radius;
    	border-radius: $radius;
    }

    // Class using the border border radius mixin
    .my-class {
      @include border-radius(5px);
    }

#### Debugging SASS ####

When the choice to use SASS was made (and not LESS), it was mainly because of the existence of tools that allow better debugging. With those tools, you’ll be able to see the right line number in the original file (not the compressed version)

- Firefox : [Firesass](https://addons.mozilla.org/fr/firefox/addon/firesass-for-firebug/) 
- Chrome : [SASS inspector](https://chrome.google.com/webstore/detail/sass-inspector/lkofmbmllpgfbnonmnenkiakimpgoamn#detail/sass-inspector/lkofmbmllpgfbnonmnenkiakimpgoamn). It can sometimes be tricky to make this plugin work.

Depending of the DEV tools you are using, some configuration may be needed to make it work. Read the documentation on the pages dedicated to the plugin for more info. In the case of the smartphone portal, we use SASS combined with COMPASS. This is the line we add in the `config.rb` to make the debugging tools work properly:

    sass_options = {:debug_info => true}

## Elements structure (HTML + CSS) ##

#### Icons ####

Icons can be reduced to 2 kinds:
- Standalone icons: symbols that don't have any text beside them, usually clickable (eg: close button on overlays)
- Decoration icons: symbols that have meaningful text beside them, used mostly just as decoration (eg: tags). the entire element (label + icon) can be clickable or not.

Depending on the kind of icon needed, the html/css may have different structure.
In both cases, a specific class for the icon needed must be defined.
- Check the icon page in the styleguide (/includes/patterns/icons in your localhost) for an icon that can suit the current need. If there isn't one, a new icon must be included in the set and a new class created.
- Use the online tool [Icomoon](https://icomoon.io/app) to import the existing set, add the additional icon, and export the new set.
- In the file _icon-font.scss define a new pseudo-element `:before` with a meaningful name, starting with "icon-".
- As the "content" value of the pseudo-element enter the css code of the new icon.
- For reference, extend the styleguide liquid file (icons.liquid) including a new row with the new icon

Once the icon class has been defined, it can be used to stylize the element depending on kind of icon (standalone or decoration).

###### Standalone icons ######
- Use a wrapping element with the class `.icon-font` and a the icon class defined earlier, eg: `.icon-zoom`
- Inside the wrapper add a span in which a meaningful explanatory text can be included, for accessibility purposes. Use the css class `.screenreader-text` for this element in order to hide it.

####### HTML: #######
    <span class="icon-font icon-zoom">
      <span class="screenreader-text">Zoom</span>
    </span>
    
###### Decoration icons ######
- Use a wrapping element with the icon class defined earlier (eg: `.icon-zoom`) and any custom class needed (eg: `.tag` or `.marker`)
- Define a `:before` pseudo-element for the custom class, and with scss `@extend .icon-font;`

####### HTML: #######
    <span class="icon-zoom tag">
      Click here to zoom
    </span>

####### SCSS: #######
    .tag:before {
		@extend .icon-font;
		}

Try to avoid, unless absolutely necessary:

- Using a background image instead of icomoon characters
- Putting icomoon characters in the html instead of css
- Overcomplicated html structure
- Writing new classes for the same purposes as [ .icon-font ]and [  .screenreader-text ] are for, or attaching similar properties to the custom class.
- Skip the inclusion of a meaningful text when not already present beside the icon.

##### A note about encoding characters for css: #####

A list of all the library icons with the corresponding ASCII codes can be found on the prototype (Elements > Icons).

CSS Unicode is different than ascii, therefore ascii codes (eg: `&#xE600;` ) won’t work as content value for `:before` pseudo-elements. The code must be “translated” in css Unicode. This can be easily done by changing the characters surrounding the code. Examples:

Example:

- ASCII code > `&#xE600;`
- CSS `:before` content value > `\e600`


