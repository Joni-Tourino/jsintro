# JavaScript: triggering actions with a click

A good introductory skill with JS (JavaScript) is learning how to trigger an action with a click. This can be used to reveal the answer to a quiz question, or allow users to choose a route through a story.

We're going to use that skill to replicate some of the functionality in the first page of [*Glastonbury quiz: Test your knowledge*](http://www.bbc.co.uk/news/uk-england-40388067), and along the way build some knowledge of:

* How to trigger a change with a click
* How to target, and change, something on the page
* How to add HTML to a page

## The `onclick` event handler

There are various ways of doing this. One is `onclick`. This is an **event handler**: it will *handle* the *event* of being clicked, normally by doing something specified by an equals operator, such as running a function.

Event handlers are placed inside HTML, rather than inside the JS script.

Here's an example - first the HTML containing that `onclick` event handler:

```html
<p>What is the capital of Japan?</p>

      <form>
         <input type="button" onclick="giveAnswer()" value="Reveal the answer" />
      </form>
```

In this case `onclick="giveAnswer()"`.  That means it looks for a function called `giveAnswer`, and runs it when clicked. The parentheses are used to pass any ingredients that the function might need to work. Even if it doesn't need any ingredients, the parentheses are still used but left empty, as in this case.

Second, then, here is some JS containing that function which has been *called* by `onclick`

```js
function giveAnswer() {
               alert("Tokyo")
}
```

Here the function simply opens an alert window with the text "Tokyo" in it.

Because the function needs to exist *before* it is called, it's best to write it in the code before any HTML. Here is a very [simple example](https://github.com/paulbradshaw/jsintro/blob/master/onclick/onclickexample.html):

```html
<script>
function giveAnswer() {
               alert("Tokyo")
}
</script>
<p>What is the capital of Japan?</p>

      <form>
         <input type="button" onclick="giveAnswer()" value="Reveal the answer" />
      </form>
```

## Changing text on the page

Alert buttons are clunky, old-fashioned ways of communicating information more commonly associated with pop-up spammy ads. We don't want this sort of user experience. So instead, a better way of doing the same thing is to change the function so that it puts some text onto the page itself.

To do that we need to *target* part of the HTML. There are a range of methods for doing this. These include:

* `getElementsByTagName`
* `getElementsByClassName`
* `GetElementById`
* `querySelectorAll`

As you might guess by the names, these target elements in a HTML page by their tag, their class, their ID, or by a selector (in the same way as a CSS selector targets elements).

In each case the method is followed by the attribute that it is targeting, in parentheses and as a string, like so:

* `getElementsByTagName('a')`
* `getElementsByClassName('subhead')`
* `GetElementById('navbar')`
* `querySelectorAll('p.story strong')`

All but one of these will return a *list* of matches - because tags, classes and selectors can be used more than once. The exception is `GetElementById`, which only returns *one* match (on the basis that an `id` attribute is only supposed to be used once on each page). This also makes it simpler to use.

Here is the function changed to use `GetElementById` to alter the page.

```js
function giveAnswer() {
               document.getElementById('changeme').textContent = "Tokyo"
}
```

You can see the key point here is `getElementById('changeme')` - this targets the HTML element with the attribute and value `id="changeme"`. We will need to alter the HTML so that something has this `id` in a moment.

First, some more explanation of the code. `getElementById` is attached to `document` with a period to specify that we are looking for that HTML element in *this* document, or HTML page: `document.getElementById('changeme')`

Now to *change* the contents of that element, more code is added: `.textContent = "Tokyo"`. The `.textContent` bit sets the text inside that element to whatever comes after an equals sign.

So, taking the line of code as a whole: it looks at the `document`, uses `.getElementById()` to look for any elements with the id `'changeme'`, and sets the `.textContent` of that element to `"Tokyo"`.

Now to add a line of HTML with that `id` attribute:

```html
<p>What is the capital of Japan?</p>
<p id="changeme"></p>
```

The full code now [looks like this](https://github.com/paulbradshaw/jsintro/blob/master/onclick/onclickexample2.html):

```html
<script>
function giveAnswer() {
               document.getElementById('changeme').textContent = "Tokyo"
}
</script>
<p>What is the capital of Japan?</p>
<p id="changeme"></p>
<form>
   <input type="button" onclick="giveAnswer()" value="Reveal the answer" />
</form>
```

### Adding formatting

There are other ways to change the content. One option is `.innerHTML`: this allows you not only to set any text, but also add extra HTML, such as formatting or attributes. It also means that you can add images and other rich media.

Let's apply this to our Glastonbury quiz.

To make sure that we have room for multiple HTML tags including paragraphs of text and any images or video, it's a good idea to change our `<p id="changeme">` to `<div id="changeme">`

The function can then be changed like so:

```js
function giveAnswer() {
                 document.getElementById('changeme').innerHTML = "<p><em>Six all-female acts have headlined the main stage. They are Adele, Beyonce, Florence and the Machine, Shakespear's Sister, Suzanne Vega and Sinead O'Connor.</em></p><img src='https://c2.staticflickr.com/4/3048/2587025047_1274a14322_o.jpg' alt='Some rights reserved by kellywritershouse'>"
  }
```

The key change here is the use of `innerHTML`. After that comes a string containing the HTML we want to 'inject' into that element with `id="changeme"`.

Note that because the string uses double quotation marks (`""`) the HTML has to use single quotation marks (`''`) when specifying attribute values like `img src='https://c2.staticflickr.com/4/3048/2587025047_1274a14322_o.jpg'`. We could do it the other way around, but not in this case because the text itself includes some apostrophes in names like Sinead O'Connor.

Again, here's the full HTML with both together:

```html
<script>
function giveAnswer() {
               document.getElementById('changeme').innerHTML = "<p><em>Six all-female acts have headlined the main stage. They are Adele, Beyonce, Florence and the Machine, Shakespear's Sister, Suzanne Vega and Sinead O'Connor.</em></p><img src='https://c2.staticflickr.com/4/3048/2587025047_1274a14322_o.jpg' alt='Some rights reserved by kellywritershouse'>"
}
</script>
<h1>Glastonbury quiz: Test your knowledge</h1>
<h3>Question 1:</h3>
<p>How many all-female acts have headlined Glastonbury's main stage in its history?<p>
<p id="changeme"></p>
<form>
   <input type="button" onclick="giveAnswer()" value="Reveal the answer" />
</form>
```

### Adding multiple answers

So far we've just had one 'reveal' button - but we can replace that with multiple 'answer' buttons by duplicating the code for one button - but keeping all three buttons in the same `<form>` tag:

```html
<form>
     <input type="button" onclick="wrongAnswer()" value="15" />
    <input type="button" onclick="wrongAnswer()" value="1" />
    <input type="button" onclick="rightAnswer()" value="6" />
  </form>
```

Note that we've also changed the name of the function being called for the three buttons: two call a function called `wrongAnswer()` and the other `rightAnswer()`.

We can now replace our single `giveAnswer()` function with two new functions. The text will remain almost the same - apart from a word which tells the user whether they got the answer right or wrong. Here's the code for each:

```js
function wrongAnswer() {
                 document.getElementById('changeme').innerHTML = "<p><strong>Wrong! </strong><em>Six all-female acts have headlined the main stage. They are Adele, Beyonce, Florence and the Machine, Shakespear's Sister, Suzanne Vega and Sinead O'Connor.</em></p><img src='https://c2.staticflickr.com/4/3048/2587025047_1274a14322_o.jpg' alt='Some rights reserved by kellywritershouse'>"
  }

function rightAnswer() {
                 document.getElementById('changeme').innerHTML = "<p><strong>Correct! </strong><em>Six all-female acts have headlined the main stage. They are Adele, Beyonce, Florence and the Machine, Shakespear's Sister, Suzanne Vega and Sinead O'Connor.</em></p><img src='https://c2.staticflickr.com/4/3048/2587025047_1274a14322_o.jpg' alt='Some rights reserved by kellywritershouse'>"
  }
```

Now this isn't the most efficient way to do things: we should really think of a way to avoid duplicating the same code. But that would involve more complex code and at this point the main objective is to get to grips with the process of triggering events with a click.

## Keeping a score

We can also use these techniques to keep a running score for the user. To do that we need to create a variable to store that score:

`var userScore = 0;`

Then in the function `rightAnswer` we can add one to that score, and assign it back to the same variable:

`userScore = userScore + 1;`

Finally, we can adjust our code so that it shows the score in the text too:

```js
document.getElementById('changeme').innerHTML = "<p><strong>Correct! </strong> Your score is "+userScore+"<em>. Six all-female acts have headlined the main stage. They are Adele, Beyonce, Florence and the Machine, Shakespear's Sister, Suzanne Vega and Sinead O'Connor.</em></p><img src='https://c2.staticflickr.com/4/3048/2587025047_1274a14322_o.jpg' alt='Some rights reserved by kellywritershouse'>"
```

Instead of one string, `innerHTML` now assigns two strings, and in the middle of that, the variable `userScore`. Those three elements are concatenated together using the `+` operator.

Alternatively, we could choose to display the score somewhere else, and add a separate line of code changing the `innerHTML` of that element.

## Tasks

* Although we have used `onclick` inside the `<input>` tag which happens to create a clickable button, it can be used on other HTML tags as well. Try using it on the `<h1>` tag by changing it to `<h1 onclick="giveAnswer()">`. Why would we normally want to avoid using it with a text tag, however?

## Further reading

[A discussion of `onclick` versus `click()` is here](https://stackoverflow.com/questions/12627443/jquery-click-vs-onclick)