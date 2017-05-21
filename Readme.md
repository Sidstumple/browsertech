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
  margin-top: -6.5em; /*This is so the page scrolls to the right position and the contact name is still visible*/
  padding-top: 6.5em;
}
```
