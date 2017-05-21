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
