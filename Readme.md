# Browser Technologies

## User Story
I want to be able to search contacts and view details.

## Basic functionality
The basic functionality of this app is searching for contacts alphabetically. This is the functionality that always works, even when Javascript or CSS styles are disabled.

## HTML
Without CSS the basic functionality of searching alphabetically still works. It's not as easy as it is **with** styles, you have to scroll to the top everytime you want to search for a new letter because the aside with letters is not fixed to the page. You can also still view details, they are automatically visible undrneath the contact's name.

![html](/img/html.gif)

## CSS 
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

## JavaScript
I enhanced the app with a search function which works through anchors. The value a user enters in the `input` tag becomes the new hash.
```javascript
searchField.addEventListener('change', function (el) {
  event.preventDefault(); //to undo the default action > refreshing the page
  var val = this.value.charAt(0).toUpperCase() + this.value.slice(1); //to make sure all queries are capitalized
  window.location.hash = val; //adds the hash to the url so it acts as an anchor
})
```

![js search](/img/js.gif)

## Accessibility
### Contrast
All elements on the app have been tested with [Lea Verou's Contrast Ratio](http://leaverou.github.io/contrast-ratio/). Contrast is very important to people with a visual impairment. It makes the difference between a good and a bad experience, so I gave it a lot of attention.

![contrast ratio](/img/contrast-ratio.png)

### Keyboard friendly
The app is accessible through the keyboard, the user can use their tab key to tab through all contacts. By pushing enter the user will get extra information about the contact.

![keyboard accessibility](/img/tab.gif)

## Affordance 
All contacts have a '+' next to them to indicate that it is expandible. All contact names have a hover state and the cursor becomes a pointer, which gives the affordance of a link which is clickable. 
I added a placeholder to the searchbar to make it's use is clearer.

## Cross browser testing
I've tested the app in IE 8, IE 11, and Microsoft Edge browsers. Of course I've also tested in Chrome, but this went as I expected, because it is the browser I always use to test and develop in. 

### Edge 
Microsoft Edge worked just fine, except for its Emulator which is supposed to show you how your website looks in older versions of IE. I knew it didn't work because my Flexbox styles were still in place. 
![details are viewed normally](/img/edge-detail.jpg)

In Edge the sticky header doesn't work, it acts as if it's set to `position: initial`. This is not a big issue though, the page still works perfectly.

![edge non sticky header](/img/edge-sticky.jpg)


### IE11 


### IE8
While testing in IE8 I noticed that the event listeners weren't working, after Googling it I found an answer on [Stack Overflow](http://stackoverflow.com/questions/9769868/addeventlistener-not-working-in-ie8).

```javascript
if (searchField.addEventListener) { // if addEventListener is a function
  searchField.addEventListener('change', function (el) {
    event.preventDefault();
    var val = this.value.charAt(0).toUpperCase() + this.value.slice(1);
    window.location.hash = val;
  })
} else { 
  searchField.attachEvent("onchange",, function (el) { // use IE's attachEvent method
    var val = this.value.charAt(0).toUpperCase() + this.value.slice(1);
    window.location.hash = val;
  })
}

```






