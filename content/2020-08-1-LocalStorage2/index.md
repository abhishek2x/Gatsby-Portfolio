---
title: "A Complete Guide to LocalStorage in JavaScript: Part-2."
path: blog/JavaScript-LocalStorage-Part-2
tags: [web, js, localstorage]
cover: ./cover.png
date: 2020-08-01
excerpt: import localStorage from ‘JavaScript’ ;

---

Before starting this article, I want to take a moment to thank you guys for so much loving and support to the first part of this article, I got an amazing response on that and many people texted me saying it was amazing and they are eagerly waiting for the second part.

So yes, your wait is over, and lets get started!

---

## Let’s recapitulate what all we have covered —

So last weekend, I released the first part of the article, “`A Complete Guide to LocalStorage in JavaScript: Part-1`” in which I introduced you with the theoretical details of the JavaScript localStorage, which were as follows —

* What is localStorage?
* Methods and Properties provided by Storage Object
* Browser compatibility with localStorage
* Limitations of localStorage

*If you want to read the full article, here’s the link —*

[`A Complete Guide to LocalStorage in JavaScript: Part-1`](https://medium.com/swlh/a-complete-guide-to-localstorage-in-javascript-ef65098e5a36)

---

## What are we going to do?

I have been thinking a lot about the project idea that we can build together in order to have a good understanding of how localStorage works.

And yes, I got this amazing idea of building a `‘LOCAL TAPAS MENU’`.

*here‘s how it looks…*

<img src="./2.png">

<br/>
<br/>

---

## Project Specifics

*Through this project you guys are going to learn about two thing:*

* Local Storage
* Event Delegation

In this project, you can add dishes in the menu, you can check in and check out the dishes and whats cool thing about this is that even if you refresh your page, you see that your menu items are still there.

This means that we are going to persist our state using localStorage.

*Whats **Event delegation?**, and why are we using this in out project?*

*Event delegation is a simple technique by which you add a single event handler to a parent element in order to avoid having to add event handlers to multiple child elements.*

In our project we have to use event delegation because when we add a dish in the Local Tapas menu, we also want to check in and check out the dish, which means we are trying to create an ‘onClick’ event listener on a element which is not even created. And you know that if an item doesn’t exist, in future you can not click in it. This problem will be addressed using the JavaScript Event Delegation.

To view the live project, you can follow this link: [`Demo`](https://abhishek2x.github.io/A-Complete-Guide-to-LocalStorage-in-JavaScript/)

---

## Development is On!

First I’ll present the gist of `HTML` and `CSS` file used in the project. You might not face any problem understanding it. All the logic for localStorange and Event Delegation is implemented in the JS file.

*I’ll explaining all the concepts involved in the js file.*

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>LocalStorage</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <!--
      Fish SVG Cred:
      https://thenounproject.com/search/?q=fish&i=589236
   -->

    <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" x="0px" y="0px"
        viewBox="0 0 512 512" enable-background="new 0 0 512 512" xml:space="preserve">
        <g>
            <path
                d="M495.9,425.3H16.1c-5.2,0-10.1,2.9-12.5,7.6c-2.4,4.7-2.1,10.3,0.9,14.6l39,56.4c2.6,3.8,7,6.1,11.6,6.1h401.7   c4.6,0,9-2.3,11.6-6.1l39-56.4c3-4.3,3.3-9.9,0.9-14.6C506,428.2,501.1,425.3,495.9,425.3z M449.4,481.8H62.6L43,453.6H469   L449.4,481.8z" />
            <path
                d="M158.3,122c7.8,0,14.1-6.3,14.1-14.1V43.4c0-7.8-6.3-14.1-14.1-14.1c-7.8,0-14.1,6.3-14.1,14.1v64.5   C144.2,115.7,150.5,122,158.3,122z" />
            <path
                d="M245.1,94.7c7.8,0,14.1-6.3,14.1-14.1V16.1c0-7.8-6.3-14.1-14.1-14.1C237.3,2,231,8.3,231,16.1v64.5   C231,88.4,237.3,94.7,245.1,94.7z" />
            <path
                d="M331.9,122c7.8,0,14.1-6.3,14.1-14.1V43.4c0-7.8-6.3-14.1-14.1-14.1s-14.1,6.3-14.1,14.1v64.5   C317.8,115.7,324.1,122,331.9,122z" />
            <path
                d="M9.6,385.2c5.3,2.8,11.8,1.9,16.2-2.2l50.6-47.7c56.7,46.5,126.6,71.9,198.3,71.9c0,0,0,0,0,0   c87.5,0,169.7-36.6,231.4-103.2c5-5.4,5-13.8,0-19.2c-61.8-66.5-144-103.2-231.4-103.2c-72,0-142.2,25.6-199,72.5l-50-47.1   c-4.4-4.1-10.9-5-16.2-2.2c-5.3,2.8-8.3,8.7-7.4,14.6l11.6,75L2.2,370.6C1.3,376.5,4.2,382.4,9.6,385.2z M380.9,230.8   c34.9,14.3,67.2,35.7,95.3,63.6c-10.1,10-20.8,19.2-31.9,27.5c-22.4-3.3-29.6-8.8-30.7-9.7c-4-5.7-11.8-7.7-18.1-4.4   c-6.9,3.6-9.5,12.2-5.9,19.1c1.9,3.5,7.3,10.3,22.4,16c-10.1,5.7-20.5,10.7-31.1,15.1C352.4,320.2,352.4,268.6,380.9,230.8z    M36.3,255.6l29.4,27.7c5.3,5,13.6,5.1,19.1,0.3c53.2-47.6,120.7-73.7,190-73.7c26.9,0,53.2,3.9,78.5,11.3   c-29.3,44.6-29.3,102,0,146.6c-25.3,7.4-51.6,11.3-78.5,11.3c-69,0-136.3-26-189.4-73.2c-2.7-2.4-13.4-6.3-19.1,0.3l-30.1,28.3   l5.7-40C42.2,293,36.3,255.6,36.3,255.6z" />
            <circle cx="398.8" cy="273.8" r="14.1" />
        </g>
    </svg>

    <div class="wrapper">
        <h2>LOCAL TAPAS</h2>
        <p></p>
        <ul class="plates">
            <li>Loading Tapas...</li>
        </ul>
        <form class="add-items">
            <input type="text" name="item" placeholder="Item Name" required>
            <input type="submit" value="+ Add Item">
        </form>
    </div>

    <script src="./script.js"></script>

</body>

</html>
```

## CSS Styling


```css
html {
  box-sizing: border-box;
  background: url("oh-la-la.jpeg") center no-repeat;
  background-size: cover;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-family: Futura, "Trebuchet MS", Arial, sans-serif;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

svg {
  fill: white;
  background: rgba(0, 0, 0, 0.1);
  padding: 20px;
  border-radius: 50%;
  width: 200px;
  margin-bottom: 50px;
}

.wrapper {
  padding: 20px;
  max-width: 350px;
  background: rgba(255, 255, 255, 0.95);
  box-shadow: 0 0 0 10px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  margin: 0;
  font-weight: 200;
}

.plates {
  margin: 0;
  padding: 0;
  text-align: left;
  list-style: none;
}

.plates li {
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  padding: 10px 0;
  font-weight: 100;
  display: flex;
}

.plates label {
  flex: 1;
  cursor: pointer;
}

.plates input {
  display: none;
}

.plates input + label:before {
  content: "⬜️";
  margin-right: 10px;
}

.plates input:checked + label:before {
  content: "🌮";
}

.add-items {
  margin-top: 20px;
}

.add-items input {
  padding: 10px;
  outline: 0;
  border: 1px solid rgba(0, 0, 0, 0.1);
}
```

## JS — The logic behind everything …

```js
const addItems = document.querySelector(".add-items");
const itemsList = document.querySelector(".plates");
const items = JSON.parse(localStorage.getItem("items")) || [];

function addItem(e) {
  e.preventDefault();
  // console.log('Hello');
  const text = this.querySelector("[name=item]").value;
  const item = {
    text,
    done: false,
  };
  // console.log(item)

  items.push(item);
  populateList(items, itemsList);
  localStorage.setItem("items", JSON.stringify(items));
  this.reset();
}

// This function is recreating the entireList each time
function populateList(plates = [], platesList) {
  platesList.innerHTML = plates
    .map((plate, i) => {
      return `
            <li>
                <input type="checkbox" data-index=${i} id="item${i}"
                 ${plate.done ? "checked" : ""}>
                <label for="item${i}">${plate.text}</label>
            </li>
        `;
    })
    .join("");
}

function toggleDone(e) {
  // console.log(e);
  // console.log(e.target);

  if (!e.target.matches("input")) return;

  // console.log(e.target);
  const el = e.target;
  const index = el.dataset.index;
  items[index].done = !items[index].done;
  localStorage.setItem("items", JSON.stringify(items));
  populateList(items, itemsList);
}

addItems.addEventListener("submit", addItem);
itemsList.addEventListener("click", toggleDone);

populateList(items, itemsList);

```

---

## So what’s Happening ?

Okay, so let’s go through the JS file displayed above. There are three main *functions* that are taking care of everything:


* addItem(…)
* populateList(…)
* toggleDone(…)


Before I go on explaining the complete function, first let me talk a bit about this `JSON.parse(…)`[line 3] and `JSON.stringify(…)`[line 46]

In localStorage, data is not saved in the same format as it is used in JS code. In you browser, open Browser `tools > Storage > Local Storage` and you will find this


<img src="./3.png">

<br/>
<br/>

We see that the data is stored in key: `value pair`. Value contains the name of the dishes that we have entered with done status.

* **JSON.parse()**

The JSON.parse() method parses a string and returns a JavaScript object.
[line 3]: In this line, we are checking if items already exist in the browser or not.It exists, then convert it into JavaScript object and display that, else return [].

* **JSON.stringify(…)**

Whenever we add anything to localStorage we need to convert it into string, this is where JSON.stringify(…) comes into picture.
[line 46]: JSON.stringify(…) is going to convert our object of arrays into JSON strings

## Functions —

* **addItem(…)**: It’s called whenever a user adds a new dish. It pushed the new dish in the array of items and calls populateList(…) to re-render new array of items.
* **populateList(…)** : It’s rendering the complete array of items.
* **toggleDone(…)**: It’s taking care of the check in and check out functionality and also calls populateList(…) to re-render the array whenever it encounters a change.

### *Congratulations!! You have successfully developed a JavaScript project implementing LocalStorage!*

# Conclusion

We did it! If you followed along with me through this whole tutorial, you should have a really good feel for localStorage now. To summarize, we have divided the article into two parts. The first part deal with the theoretical understanding and conceptual details of how to implement localStorage in JavaScript. The second part(this article) covers the implementation and in-depth explanation of localStorage and also develop a cool Project.

This article should have given you a good understanding of localStorage in JavaScript. There is much more to learn and improve, but I hope you feel confident delving in and playing around with localStorage yourself now.

* [`View Source on GitHub`](https://github.com/abhishek2x/A-Complete-Guide-to-LocalStorage-in-JavaScript)
* [`Wiki of the repository`](https://github.com/abhishek2x/A-Complete-Guide-to-LocalStorage-in-JavaScript/wiki)

Please let me know if anything was unclear, or if there’s anything else you’d like to see in this or a subsequent article. Feel free to reach out to me anytime if you want to discuss something. I would be more than happy if you send your feedback, suggestions.

---

#### *Thanks a lot for reading till end. You can contact me in case if you need any assistance:*

**Web:** https://portfolio.abhisheksrivastava.me/

**Instagram:** https://www.instagram.com/theprogrammedenthusiast/

**LinkedIn:** https://www.linkedin.com/in/abhishek-srivastava-49482a190/

**Github:** https://github.com/abhishek2x

**Email:** abhisheksrivastavabbn@gmail.com



Link to published article: [`Medium`](https://medium.com/swlh/a-complete-guide-to-localstorage-in-javascript-part-2-115ecae5e00c)