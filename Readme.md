# Browser Technologies
View live version on: https://browsertech.herokuapp.com/

### Contents:
1. [User Story](#user-story)
2. [Basic Functionality](#basic-functionality)
3. [Features](#features)<br>
  3.1 [HTML](#html)<br>
  3.2 [CSS](#css)<br>
  3.3 [Javascript](#javascript)
4. [Accessibility](#accessibility)
5. [Accessibility](#accessibility)<br>
  5.1  [Contrast](#contrast)<br>
  5.2 [Keyboard friendly](#keyboard)
6. [Affordance](#affordance)
7. [Crossbrowser testing](#crossbrowser-testing)<br>
  7.1 [Edge](#edge)<br>
  7.2 [IE11](#ie11) <br>
  7.3 [IE8](#ie8)<br>

## Summary
This app was made for the course Browser Technologies of the Minor Web Development at the University of Applied Sciences Amsterdam. Browser Technologies helped us to view our websites and apps from a different angle, from a kind of dark place, sometimes literally, where a lot users of the internet reside. We were forced to face the facts: not everyone is viewing your app on the newest browser, not everyone has the fastest internet connection, and not everyone can see. The most important lesson I took from this was backwards compatability, start with a core- or basic functionality and make every step an enhancement, not a neccessity. 

## [User Story](#user-story)
I want to be able to search contacts and view details.

## [Basic Functionality](#basic-functionality)
The basic functionality of this app is searching for contacts alphabetically. This is the functionality that always works, even without Javascript & CSS, and even in IE8.

## [Features](#features)
+ Searching alphabetically
+ Searching by query
+ Viewing details by clicking on a contact
+ Sticky position headers that stick to the top of the page while scrolling

### [HTML](#html)
Without CSS the basic functionality of searching alphabetically still works. It's not as easy as it is **with** styles, you have to scroll to the top everytime you want to search for a new letter because the aside with letters is not fixed to the page. You can also still view details, they are automatically visible undrneath the contact's name.

![html](/img/html.gif)

### [CSS](#css) 
With CSS i've added `position: fixed` to the aside, this makes it easier for the user to search the contacts alphabetically. I also set `height: 0` to the details of every contact and made them visible when they are targeted:
```css
:target  > .expand {
  overflow: visible;
  height: 4.5em;
  transition: 1s;
}
```
Because anchors require no javascript they are very useful in this app.

![css anchors](/img/css.gif)

### [Javascript](#javascript)
I enhanced the app with a search function which works through anchors. The value a user enters in the `input` tag becomes the new hash.
```javascript
searchField.addEventListener('change', function (el) {
  event.preventDefault(); //to undo the default action > refreshing the page
  var val = this.value.charAt(0).toUpperCase() + this.value.slice(1); //to make sure all queries are capitalized
  window.location.hash = val; //adds the hash to the url so it acts as an anchor
})
```

![js search](/img/js.gif)

## [Accessibility](#accessibility)
### [Contrast](#contrast)
All elements on the app have been tested with [Lea Verou's Contrast Ratio](http://leaverou.github.io/contrast-ratio/). Contrast is very important to people with a visual impairment. It makes the difference between a good and a bad experience, so I gave it a lot of attention.

![contrast ratio](/img/contrast-ratio.png)

### [Keyboard friendly](#keyboard)
The app is accessible through the keyboard, the user can use their tab key to tab through all contacts. By pushing enter the user will get extra information about the contact.

![keyboard accessibility](/img/tab.gif)

## [Affordance](#affordance)
All contacts have a '+' next to them to indicate that it is expandible. All contact names have a hover state and the cursor becomes a pointer, which gives the affordance of a link which is clickable. 
I added a placeholder to the searchbar to make it's use is clearer.

## [Crossbrowser testing](#crossbrowser-testing)
I've tested the app in IE 8, Firefox, and Microsoft Edge browsers. Of course I've also tested in Chrome, but this went as I expected, because it is the browser I always use to test and develop in. 

### [Edge](#Edge)
Microsoft Edge worked just fine, except for its Emulator which is supposed to show you how your website looks in older versions of IE. I knew it didn't work because my Flexbox styles were still in place. 
![details are viewed normally](/img/edge-detail.jpg)

In Edge the sticky header doesn't work, it acts as if it's set to `position: initial`. This is not a big issue though, the page still works perfectly.

![edge non sticky header](/img/edge-sticky.jpg)

### [Firefox](#firefox)
Everything works in firefox.
![firefox](/img/firefox)

### [IE8](#IE8)
Everything broke in IE8, here is a list and I'm sure I forgot something:
- `.innerHTML`
- `addEventListener`
- `parentNode`
- `event.preventDefault`
- `:target`
- `classList`

#### `.innerHTML`
Appearently you cannot use `.innerHTML` for html tags in IE8, I fixed this with `createElement` and `appendChild`:
```javascript
var form = document.createElement('form');
var input = document.createElement('input');
input.setAttribute('type', 'search');
input.setAttribute('class', 'search');
input.setAttribute('placeholder', 'Search contacts');
input.setAttribute('id', 'search-field');
var submitButton = document.createElement('button');
submitButton.appendChild(document.createTextNode('Zoek'));
submitButton.setAttribute('id', 'search-button');
submitButton.setAttribute('type', 'submit');

search.appendChild(form);
form.appendChild(input);
form.appendChild(submitButton);
```

#### `addEventListener`
While testing in IE8 I noticed that the event listeners weren't working, after Googling it I found a fallback on [Stack Overflow](http://stackoverflow.com/questions/9769868/addeventlistener-not-working-in-ie8).

```javascript
if (searchField.addEventListener) { // if addEventListener is a function
  searchField.addEventListener('change', function (el) {
    event.preventDefault();
    var val = this.value.charAt(0).toUpperCase() + this.value.slice(1);
    window.location.hash = val;
  })
} else { 
  searchField.attachEvent("onchange", function (el) { // use IE's attachEvent method
    var val = this.value.charAt(0).toUpperCase() + this.value.slice(1);
    window.location.hash = val;
  })
}

```

#### `parentNode`
Then I found out `parentNode` wasn't working, I found a different way to get the value of the input:
```javascript
  document.getElementById('search-field').value.charAt(0).toUpperCase() + document.getElementById('search-field').value.slice(1); // to make every query be capitalized.
```

#### `event.preventDefault()`
Not supported, found a fallback:
```javascript
  el.returnValue = false; // el is the parameter of the event function
```

#### `:target`
The CSS `:target` selector is not supported in IE8. All details from contacts were viewed by using a target selector. I first tried to use the @supports feature detection of CSS, but this of course didn't work because `:target` is not a css rule, it's a selector.
I decided to fix it with Javascript and added a class.

#### `classList` 
ClassList is also not supported in IE8, I fixed this by using `className`:
```javascript
  var details = document.querySelectorAll('#details');
  for (i=0; i<details.length; i++) {
    details[i].className += 'internet-explorer';
  }
```


### Screenreader and devicelab testing
![Testing in device lab](/img/devicelab.gif)

The website worked on every device, on some devices the website was slightly zoomed in, this was probably because I used `vw` for the width of the website. There were also some problems with the target selector, it sometimes made the details appear above the viewport. The rest of the website worked fine and as expected.

![Testing in device lab](/img/devicelab.jpg)

