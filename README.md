## Hour 12
## JavaScript and CSS
### What You’ll Learn in This Hour:

> * Separating style from content
> * The DOM style property
> * Retrieving styles
> * Setting styles
> * Accessing classes using className
> * The DOM styleSheet object
> * Enabling, disabling, and switching stylesheets in JavaScript
> * Changing the mouse cursor

In the early days of the World Wide Web, pages were all about their text content. Early browsers had rudimentary support for graphic effects—some didn’t even support images. Styling a web page was largely a matter of using the few style-related attributes and tags allowed by the early incarnations of HTML.

Things improved markedly with the introduction of browser support for Cascading Style Sheets (CSS), which allowed the styling of a page to be treated independently from its HTML markup.

Earlier in the book, you learned how to edit the structure of your page using JavaScript’s DOM methods. However, JavaScript can also be used to access and amend CSS styles for the current page. In this hour you learn how.

### A Ten-Minute CSS Primer
If you’ve decided to learn JavaScript there’s a pretty good chance that you’re already familiar with CSS styling. Just in case it’s managed to pass you by, let’s review the basics.

#### Separating Style from Content
Before CSS came along, most styling in HTML pages was carried out using HTML tags and/or their attributes. To change the font color of a piece of text, for example, you would have used something like this:

`<p><font color="red">This text is in red!</font></p>`

This was pretty awful for a number of reasons:

* Every single piece of text in the page that we wanted to be colored red had to be marked up with these extra tags.
* The created style could not be carried over to other pages; they too had to be marked up individually with additional HTML.
* To later change pages’ styles, you had to edit each and every page and sift through the HTML, changing every style-related tag and attribute individually.
* With all this extra markup, the HTML became very hard to read and maintain.

CSS attempts to separate the styling of an HTML element from the markup function of that element. This is done by defining individual style declarations and then applying these to HTML elements or collections of elements.

You can use CSS to style the visual properties of a page element, such as color, font, and size, as well as format-related properties such as positioning, margins, padding, and alignment.

Separating style from content in this way brings with it a lot of benefits:

* Style declarations can be applied to more than one element or even (when using external stylesheets) more than one page.
* Changes to style declarations affect all associated HTML elements, making updating your site’s style more accurate, quick, and efficient.
* Sharing styles encourages more consistent styling through your site.
* HTML markup is clearer to read and maintain.

#### CSS Style Declarations
The syntax of CSS style declarations is not unlike that of JavaScript functions. Suppose you want to declare a style for all paragraph elements in a page, causing the font color inside the paragraphs to be colored red:

```CSS
p {
    color: red
}
```
You can apply more than one style rule to your chosen element or collection of elements, separating them with semicolons:

```CSS
p {
    color: red;
    text-decoration: italic;
}
```

Since you have used the selector `p`, the preceding style declarations affect every paragraph element on the page. To select just one specific page element, you can do so by using its ID. To do so, the selector you use for your CSS style declaration is not the name of the HTML element, but the ID value prefixed by a hash character. For instance, the HTML element

`<p id="para1">Here is some text.</p>`

could be styled by the following style declaration:

```CSS
#para1 {
    font-weight: bold;
    font-size: 12pt;
    color: black;
}
```
To style multiple page elements using the same style declaration, you can simply separate the selectors with commas. The following style declaration affects all `<div>` elements on the page, plus whatever element has the id value para1:

```CSS
div, #para1 {
    color: yellow;
    background-color: black;
}
```

Alternatively, you can select all elements sharing a particular class attribute, by prefixing the class name with a dot to form your selector:

```CSS
<p class="info">Welcome to my website.</p>
<span class="info">Please log in or register using the form
below.</span>
```

We can style these elements with one declaration:

```CSS
.info {
    font-family: arial, verdana, sans-serif;
    color: green;
}
```
#### Tip

> Styles defined in external stylesheets have the advantage that they can easily be applied to multiple pages, whereas styles defined within the page can’t.

#### Note

> In addition to the syntax
> `myNode.style.width`
> you can also use the equivalent
> `myNode.style["width"]`
> This is sometimes necessary, such as when passing a
> property name as a variable:
> ```JavaScript
var myProperty = "width";
myNode.style[myProperty] = "200px";
```

#### Note

> In Hour 13, “Introducing CSS3,” you’ll read about another way to access style properties in JavaScript that avoids the limitation of only working for inline styles.

#### Note

> CSS contains many properties with names that contain hyphens, such as background-color, font-size, text-align, and so on. Since the hyphen is not allowed in JavaScript property and method names, we need to amend the way these properties are written. To access such a property in JavaScript, remove the hyphen from the property name and capitalize the character that follows, so font-size becomes fontSize, text-align becomes textAlign, and so on.

#### Where to Place Style Declarations
Somewhat similarly to JavaScript statements, CSS style declarations can either appear within the page or be saved in an external file and referenced from within the HTML page.

To reference an external stylesheet, normal practice is to add a line to the page `<head>` like this:

```HTML
<link rel="stylesheet" type="text/css" href="style.css" />
```
Alternatively, you can place style declarations directly in the `<head>` of your page between `<style>` and `</style>` tags:

```CSS
<style>
    p {
        color: black;
        font-family: tahoma;
    }
    h1 {
        color: blue;
        font-size: 22pt;
    }
</style>
```

Finally, it’s possible to add style declarations directly into an HTML element by using the style attribute:

`<p style="color:red; font-size: 12px;">Please see our terms of service.</p>`

### The DOM style Property
You saw in previous hours how the HTML page is represented by the browser as a DOM tree. The DOM nodes—individual “leaves and branches” making up the DOM tree—are objects, each having its own properties and methods.

You’ve seen various methods that allow you to select individual DOM nodes, or collections of nodes, such as `document.getElementById()`.

Each DOM node has a property called style, which is itself an object containing information about the CSS styles pertaining to its parent node. Let’s see an example:

```HTML
<div id="id1" style="width:200px;">Welcome back to my site.</div>
<script>
    var myNode = document.getElementById("id1");
    alert(myNode.style.width);
</script>
```

In this case the alert would display the message “200px.”

Unfortunately, while this method works fine with inline styles, if you apply a style to a page element via a `<style>` element in the head of your page, or in an external stylesheet, the DOM style object won’t be able to access it.

The DOM style object, though, is not read-only; you can set the values of style properties using the style object, and properties you’ve set this way will be returned by the DOM style object.

#### Try It Yourself: Setting Style Properties

Let’s write a function to toggle the background color and font color of a page element between two values, using the DOM style object:

function toggle() {
    var myElement = document.getElementById("id1");
    if(myElement.style.backgroundColor == 'red') {
        myElement.style.backgroundColor = 'yellow';
        myElement.style.color = 'black';
    } else {
        myElement.style.backgroundColor = 'red';
        myElement.style.color = 'white';
    }
}

The function toggle() first finds out the current background-color CSS property of a page element, and then compares that color to red.

If the background-color property currently has the value of red, it sets the style properties of the element to show the text in black on a yellow background; otherwise, it sets the style values to show white text on a red background.

We use this function to toggle the colors of a <span> element in an HTML document.

The complete listing is shown in Listing 12.1.



LISTING 12.1 Styling Using the DOM style Object



Create the HTML file in your editor and try it out.

You should see that when the page originally loads, the text is in the default black and has no background color. That happens because these style properties are initially not set in the <style> instructions in the page head, as an inline style, or via the DOM.

Executing when the button is clicked, the toggle() function checks the current background color of the <span> element. On finding that its value is not currently red, toggle() sets the background color to red and the text color to white.

The next time the button is clicked, the test condition

if(myElement.style.backgroundColor == 'red')

returns a value of true, causing the colors to be set instead to black on a yellow background.

Figure 12.1 shows the program in action.

Image
FIGURE 12.1 Setting style properties in JavaScript

Accessing Classes Using className
Earlier in this hour we discussed separating style from content, and the benefits that this can bring.

Using JavaScript to edit the properties of the style object, as in the previous exercise, works well—but it does carry with it the danger of reducing this separation of style and content. If your JavaScript code routinely changes elements’ style declarations, the responsibility for styling your pages is no longer lodged firmly in CSS. If you later decide to change the styles your JavaScript applies, you’ll have to go back and edit all of your JavaScript functions.

Thankfully, we have a mechanism by which JavaScript can restyle pages without overwriting individual style declarations. By using the className property of the element, we can switch the value of the class attribute and with it the associated style declarations for the element. Take a look at Listing 12.2.

LISTING 12.2 Changing Classes Using className



The <style> element in the page <head> lists style declarations for two classes, classA and classB. The JavaScript function toggleClass() uses similar logic to the earlier function toggle() of Listing 12.1, except that toggleClass() does not work with the element’s style object. Instead, toggleClass() gets the class name associated with the <div> element and switches its value between classA and classB.

Figure 12.2 shows the script in action.

Image
FIGURE 12.2 Switching classes in JavaScript

Note

As an alternative to using className, you could try setting the class attribute for an element to the value classA by using

element.setAttribute("class", "classA");

Unfortunately, various versions of Internet Explorer have trouble when trying to set the class attribute, but work fine with className. The statement

element.className = "classA";

seems to work in all browsers.

The DOM styleSheets Object
The styleSheets property of the document object contains an array of all the stylesheets on the page, whether they are contained in external files and linked into the page head, or declared between <style> and </style> tags in the page head. The items in the styleSheets array are indexed numerically, starting at zero for the stylesheet appearing first.

Tip

You can access the total number of spreadsheets on your page by using

document.styleSheets.length

Enabling, Disabling, and Switching Stylesheets
Each stylesheet in the array has a property called disabled, containing a value of Boolean true or false. This is a read/write property, so we are able to effectively switch individual stylesheets on and off in JavaScript:

document.styleSheets[0].disabled = true;
document.styleSheets[1].disabled = false;

The preceding code snippet “switches on” the second stylesheet in the page (index 1) while “switching off” the first stylesheet (index 0).

Listing 12.3 has a working example. The script on this page first declares the variable whichSheet, initializing its value at zero:

var whichSheet = 0;

This variable keeps track of which of the two stylesheets is currently active. The second line of code initially disables the second of the two stylesheets on the page:

document.styleSheets[1].disabled = true;

The function sheet(), which is attached to the onClick event handler to the button on the page when the page loads, carries out three tasks when the button is clicked:

Disable the stylesheet whose index is stored in variable whichSheet:
document.styleSheets[whichSheet].disabled = true;

Toggle variable whichSheet between one and zero:
whichSheet = (whichSheet == 1) ? 0 : 1;

Enable the stylesheet corresponding to the new value of whichSheet:
document.styleSheets[whichSheet].disabled = false;

The combined effect of these activities is to toggle between the two active stylesheets for the page. The script is shown in action in Listing 12.3 and Figure 12.3.

Image
FIGURE 12.3 Switching stylesheets with the styleSheets property

LISTING 12.3 Toggling Between Stylesheets Using the styleSheets Property



Try It Yourself: Selecting a Particular Stylesheet

Video 12.2—Selecting a Particular Stylesheet

Having your stylesheets indexed by number doesn’t make it easy to select the stylesheet you need. It would be easier if you had a function to allow you to title your stylesheets and select them by their title attribute.

You need your function to respond in a useful manner if you ask for a stylesheet that doesn’t exist; you want it to maintain the previous stylesheet and send the user a message.

First, declare a couple of variables and initialize their values:

var change = false;
var oldSheet = 0;

The Boolean variable change keeps track of whether you’ve found a stylesheet with the requested name; once you do so, you change its value to true, indicating that you intend to change stylesheets.

The integer oldSheet, originally set to zero, will eventually be assigned the number of the currently active sheet; in case you don’t find a new stylesheet matching the requested title, you set this back to active before returning from the function.

Now you need to cycle through the styleSheets array:

for (var i = 0; i < document.styleSheets.length; i++) {
    ...
}

For each stylesheet:

If you find that this is the currently active stylesheet, store its index in the variable oldSheet:
if(document.styleSheets[i].disabled == false) {
    oldSheet = i;
}
As you cycle through, make sure all sheets are disabled:
document.styleSheets[i].disabled = true;
If the current sheet has the title of the requested sheet, make it enabled by setting its disabled value to false, and immediately set your variable change to true:
if(document.styleSheets[i].title == mySheet) {
    document.styleSheets[i].disabled = false;
    change = true;
}
When you’ve cycled through all sheets, you can determine from the state of the variables change and oldSheet whether you are in a position to change the stylesheet. If not, reset the prior stylesheet to be enabled again:

if(!change) document.styleSheets[oldSheet].disabled = false;

Finally, the function returns the value of variable change—true if the change has been made, or false if not.

The code is listed in Listing 12.4. Save this code in an HTML file and load it into your browser.

LISTING 12.4 Selecting Stylesheets by Title



The small function sheet() is added to the button’s onClick event handler when the page loads. Each time the button is clicked, sheet() prompts the user for the name of a stylesheet:

var sheetName = prompt("Stylesheet Name?");

Then it calls the ssEnable() function, passing the requested name as an argument.

If the function returns false, indicating that no change of stylesheet has taken place, you alert the user with a message:

if(!ssEnable(sheetName)) alert("Not found - original stylesheet retained.");

The script is shown operating in Figure 12.4.

Image
FIGURE 12.4 Selecting a new stylesheet by name

Summary
In this hour you learned a number of ways in which JavaScript can be put to work on the CSS styles of your page. You learned how to use the style property of page elements, how to work with CSS classes, and how to manipulate entire stylesheets.

Q&A
Q. Is it possible for JavaScript to work with individual CSS style rules?

A. Yes it is, but at the time of writing this does not work very well cross-browser. Mozilla browsers support the cssRules array, while Internet Explorer calls the equivalent array Rules. There is also considerable difference among browsers in how the notion of a “rule” is interpreted. It’s to be hoped that future browser versions will resolve these differences.

Q. Is it possible to alter the mouse cursor in JavaScript?

A. Yes, it is. The style object has a property called cursor that can take various values. Popular cursors include the following:

Crosshair—Pointer renders as a pair of crossed lines like a gun sight.
Pointer—Usually a pointing finger.
Text—Text entry caret.
Wait—The program is busy.
Quiz
Let's get started!







Exercises
Edit the program of Listing 12.1 to change other style properties such as font face and decoration, element borders, padding, and margins.
Change the program of Listing 12.4 so that some of the stylesheets are externally linked, rather than situated between <style> and </style> tags in the page <head>. Does everything work the same?
