# Match Game SkillBuilder
## Contents
- Introduction
- Function Overview
    - Stage 1
    - Stage 2
- Functions In Depth
- Included Function
- Test Suite

## Introduction
Welcome to the Match Game SkillBuilder!
	
Your task is to code out the game's functionality by completing the functions we've outlined below. We've left you plenty of comments as well as a suite of tests that will help you get the job done. Keep in mind that the functions build upon each other so **YOU SHOULD COMPLETE EACH FUNCTION IN ORDER.**

## Function Overview
Here is an overview of the functions you'll be creating: 


### STAGE 1: 
- `createNewCard()` - This function will create a new div element that represents a card and return it.


- `appendNewCard(parentElement)` - This function will use `createNewCard()` to create a new card element and attach it to the element supplied as an argument to the `parentElement` parameter.   


- `shuffleCardImageClasses()` - The included CSS file contains styles for 6 image classes named `image-1` through `image-6`. This function will return an array that contains each name **TWICE** (i.e. 12 strings) randomly ordered.

_NOTE: For the `shuffleCardImageClasses()` function, it's important to use the correct class names as the CSS is responsible for adding the images to our card faces. If you use different class names (e.g. "img3" instead of `image-3`) the card faces will not show up!_


### STAGE 2: 
- `createCards(parentElement, shuffledImageClasses)` - This function is the one that will actually create all 12 cards and append them to the DOM. It will also return an array of 12 card objects to manage and compare the cards in our code.

- `doCardsMatch(cardObject1, cardObject2)` - This function accepts two card objects as arguments and returns true if their image classes match, false otherwise.

- `incrementCounter(counterName, parentElement)` - This function increments the counter supplied as an argument to `counterName`. Then it updates its `parentElement` in the DOM to display the change to the user.

- `onCardFlipped(newlyFlippedCard)` - This function handles the logic of a card being flipped (e.g is this the first or second card flipped? Do the cards match or not?)

- `resetGame()` - This function removes all cards from the card container, resets the flip and match counters, and sets up a new game. 


## Functions In Depth

### `createNewCard()`
This function should create and return a new DOM element that represents a card. The structure of the card should be as follows: 
```html
	<div class="card">
		<div class="card-down"></div>
		<div class="card-up"></div>
	</div>
``` 
Note that we aren't "attaching" this element to the DOM yet but, when we do (in the `appendNewCard` function), the included CSS will display this HTML structure as a card. 

**TEST:** `createNewCardTest()`
<hr>

### `appendNewCard(parentElement)`
This function will call `createNewCard()` and attach the returned card to the DOM so it actually shows up on the page (without a picture for now). 

The card should be attached as a "child" of the parameter `parentElement`.  Here is what `parentElement` will look like at the beginning of the function:

```html
<div id="card-container">
</div>
```

And here is what it should look like at the end:

```html
<div id="card-container">
	<div class="card">
		<div class="card-down"></div>
		<div class="card-up"></div>
	</div>
</div>
```

**TEST:** `appendNewCardTest()`
<hr>

### `shuffleCardImageClasses()`
The `style.css` file has styles for six classes named `image-1` through `image-6`. When applied to a card, these classes will make the card face show the corresponding image when flipped. 
	
This function is all about returning a "shuffled" array with two of each of these class names as strings. In the next function, we'll combine this and `appendNewCard()` to create and assign random pictures to cards.

You'll use a library called "underscore.js" to shuffle the array. The library is called underscore because it uses an "_" character as an object to contain helper methods. You'll need to load underscore.js in your HTML via a CDN link then open up the documentation linked below to learn how to use the `shuffle` method. Make sure to paste the CDN script tag **ABOVE** the other scripts in `index.html`— otherwise it won't work! 

- [Underscore.js CDN Link](https://cdnjs.com/libraries/underscore.js) 
- [_.shuffle Documentation](https://www.tutorialspoint.com/underscorejs/underscorejs_shuffle.htm)

*NOTE: Ignore the "require" syntax shown in the documentation above as this is for non-browser environments. The `_` variable will automatically be available to you if you properly link the CDN in your project.*  

**TEST:** `shuffleCardImageClassesTest()`
<hr>

### `createCards(parentElement, shuffledImageClasses)`
This function is going to: 

1. Create all 12 cards with random images and append each one to the DOM
2. Return an array of custom card objects that we can use to organize our cards on the Javascript side, separate from the HTML document / DOM. 

To complete part 1 we'll loop 12 times. Each iteration we'll create a card that is a child of `parentElement` (using `appendNewCard()`) and add an image class name to the element from the **ALREADY SHUFFLED** `shuffledImageClasses` parameter.

To complete part 2 we'll create a new variable to store an array of custom card objects. Then we'll push a new object to the array each iteration of the loop. The custom card object should have the following properties: 

- `index` -- Which iteration of the loop this is.
- `element` -- The DOM element for the card.
- `imageClass` -- The string of the image class on the card. 

Finally, we'll need return the array so we can use the data elsewhere in our program.  

**TEST:** `createCardsTest()`
<hr>

### `doCardsMatch(cardObject1, cardObject2)`
Once we know what our card objects look like, we'll write a function to determine if two cards match. Remember the properties of our card objects from the `createCards()` function: `index`, `element`, and `imageClass`.

This function should return `true` when both cards (`cardObject1` and `cardObject2`) have the same class name and `false` otherwise. 

**TEST:** `doCardsMatchTest()`
<hr>

### `incrementCounter(counterName, parentElement)`
This function will add one to a counter being displayed on the webpage (either the number of cards flipped or the number of cards matched) --  the trick is that we use an "object dictionary" to store the two counters (flips/matches).

Basically, `counters["flips"]` will store the flip count and `counters["matches"]` will store the match count. The name `"flips"`, `"matches"`, or something else will be supplied as an argument to the `counterName` parameter. The `counters` object is a global variable declared in the starter code.

The `parentElement` parameter is the DOM element that shows the counter in the HTML (e.g. `<span id="flip-count">`). The `innerHTML` of this element determines what value is displayed for the counter.

**TEST:** `incrementCounterTest()`
<hr>

### `onCardFlipped(newlyFlippedCard)`
This function will be invoked each time the user flips a card. It has one parameter: `newlyFlippedCard` which is the custom card object associated with whichever card has just been flipped. This function should handle the majority of our game's logic: 

- Is this the first or second card flipped in a sequence?
- Do the cards match? 
- Is the game over?

First off, you'll use the `incrementCounter` function to add one to the flip counter's UI. 

Once the flip counter is incremented, you should check if this is the first card that has been flipped (if `incrementCounter` is null). If this is the first card flipped, reassign the value of `lastCardFlipped` to the card object that was just flipped and exit the function with a `return`. 

If two cards are flipped but they *DON'T* match, remove the "flipped" class from each card's DOM element, set `lastCardFlipped` back to null, and return. **Remember that `newlyFlippedCard` and `lastCardFlipped` are both objects made with the `createCards` function. This means that, to access each card's class list, you must access the card object's `.element` property first!**

If two cards are flipped and they match, you should increment the match counter and optionally add a "glow" effect to the matching cards. Play either the win audio or match audio based on whether the user has the number of matches needed to win or not. Both audio files have been loaded in `provided.js` and are stored in variables you can use in your script. You can access them with `matchAudio` and `winAudio`, respectively.

Finally, reset `lastCardFlipped` to null.

**No testing is provided for this function.**

### `resetGame()`
This function will be called when the user clicks the Reset button at the bottom of the page. 

Start by removing all children from the card-container `<div>`. For more information about how to accomplish this, see the Example section of the [MDN removeChild documentation](https://developer.mozilla.org/en-US/docs/Web/API/Node/removeChild#examples). Keep an eye out for "To remove all children from an element".

Next, reset the flip and match counts displayed in the HTML to zero. Then, reset the `counters` dictionary to an empty object.

Lastly, you should set `lastCardFlipped` back to `null` and use the included `setUpGame` function to set up a new game.

**No testing is provided for this function.**

## Included Function
### `setUpGame()`
This function sets up the game by calling the `createCards()` function and adding an `onclick` to new each card created.

## Test Suite
We have provided you with a suite of tests to test the functionality of your code. If a test fails, it will log a description of the error to the console and provide you with helpful links to resources in TBHQ and elsewhere on the web to help you debug. 

### Included tests: 
- `createNewCardTest()`
- `appendNewCardTest()`
- `shuffleCardImageClassesTest()`
- `createCardsTest()`
- `doCardsMatchTest()`
- `incrementCounterTest()`