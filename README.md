# KWK Instructor Training 2 - JavaScript

JavaScript is **the** language of the front end. Besides the mechanics that can create solutions to logic problems (which you'll basically see in all languages), JavaScript allows us to do two important things for web applications:

1. Render data on the page and take user input
2. Make requests for data based on user input

Let's talk about what each of those mean, and how they are related.


## DOM Manipulation

Document Object Model (DOM) Manipulation is at the heart of User Interaction (UI). Think of the DOM as what we, as users, see in the browser. Any time something is rendered on the page, something changes when a button is clicked, etc., the _DOM is being manipulated_, or changed.  

Think of opening up the Instagram app - it takes a second or two to load up, then you see `cards` showing a photo, the heart/comments/share/save icons, number of likes, caption, and comments. They weren't just waiting for you to open your app - they were rendered when you opened it (asked it) to do so. When you click the heart icon, it changes from a black outlined/no fill icon, to a red icon, and the number of likes increases by 1.

All of those things you SEE are DOM manipulation.

Today, we will use `jQuery`, a JavaScript library, to manipulate the DOM. A library is code, small or huge, that someone else wrote, which makes our lives as developers easier by doing some of the heavy lifting for us! jQuery is older but still popular and widely used.


## Network Requests

First, let's break down each part of this phrase:
- NETWORK: A system of computers that are joined together so that they can communicate
- REQUEST: An act of asking politely or formally for something

Everything you do on the internet is making (probably) _many_ network requests.

Let's go back to Instagram - that 'second or two' it takes to load up is cause by the network requests that are being sent (and the data being sent back in response) to the Instagram server. When you open the application, a network request `fires` - your computer says "Hey Instagram, _____ is logging in and wants to see their feed. Can you send it back to me please?". Once the data is sent back, we use DOM Manipulation to show it to the user. When you click that heart icon, your computer sends a request to Instagram and says "Hey Instagram, this user liked this specific photo - can you note that in your data?". The data is updated, then your computer receives word that the data was updated, so it manipulates the DOM and changes the heart icon to red, and increases the count of likes.

Whether you know it or not, you are probably asking your phone or computer to make network requests about 16 hours/day.

Today, we will use a tool called `fetch` to make requests. It is a built in JavaScript function and it has this name because it 'fetches' data. Keep in mind, in addition to asking for data, this includes sending, updating, and deleting date.

## Putting these concepts into an App

### Setup
Log into your [CodePen account](http://codepen.io/), and go to [this codepen](https://codepen.io/ameseee/pen/rKqaLE). In the navigation bar on the top right corner, you'll see a button that says "fork" - click that, and you will create a copy of the project, but on your account. This means that any changes you make will _not_ affect the original, but will be saved to yours (so long as you are periodically clicking 'save').

### Check out what we're starting with
Take a few minutes to familiarize yourself with the CodePen platform, then the code in the HTML and CSS files, and ultimately, what they are producing in the 'browser'.

If you don't understand every single part of the HTML or CSS, that's ok for this session. Feel free to go back through this and ask questions in your channel or DM Amy if you'd like to follow up on it.

Let's also talk about what is happening in the JavaScipt file. The code in lines 1-13 is just some starter code so we all use the same naming. Those are all JavaScript _functions_. Lines 15-17 is the function that will fire as soon as the page loads! It is saying "Hey website, as soon as you are loaded up in the browser, please call the `getHedgehogs` function." Line 19 is an `event listener` - it is sitting there, waiting for the un-invite button on any given invitee to be clicked on, and when that happens, it is going to call the `unInviteHedgeHog` function. Don't worry about the details of how that works right now.

### What is our goal?
There will be several pieces to making this a fully functional, very important, Hedgehog Party Tracking App.
- Show the names of all the hedgies that have been invited (we already have two who have signed up!)
- Allow the party planner to invite a new hedgie/hedgie family
- Allow the party planner to un-invite a hedgie/hedgie family

If you visit `https://hedgehog-party.herokuapp.com/api/v1/invites` you can see the database, or API, we will be making requests to. As you can see, it already has some hedgies on the invite list - but we only see `Name | 15 | Allergies` - which is actually hard-coded into the HTML. We will take it out _later_.

Now that we see that data - let's make sure we know **how** we will dig into it. This is an array of objects. We will need to iterate over the array, then use dot or bracket notation to access the values of the key/value pairs within each object.

## Time to write CODE!

We already did a LOT of work just to set ourselves up to be successful to code - if you are NOT spending time planning and thoroughly understanding the vision of an application, you're doing it wrong. And if you think coding is anything like this:

![matrix code](./coder.png)

think again, my friends😜 You should put a lot of thought and work and design into an app before writing a single line of code.

### Show the Invitees
To show the name/number of hoglets/allergies for **each** hedgehog in our database, we need to ASK the database for that information! Once we get that data, we will use DOM Manipulation to render it (show it to the user).

We were given the following code - this is a function, called `getHedgehogs`, which right now, will **only** do one small job - clear out anything in the HTML within the tag that has an ID of `hedgehog-info`.

```js
const getHedgehogs = () => {
  $('#hedgehog-info').html('');

};
```

Since this is the function that is called when the page is loaded, this seems like the place that we want to ask the database for that list.

```js
const getHedgehogs = () => {
  $('#hedgehog-info').html('');

  fetch("https://hedgehog-party.herokuapp.com/api/v1/invites")  
    .then(response => response.json)
    .then(hedgehogs => console.log(hedgehogs))
};
```

Open up the devleoper tools in the browser by clicking `cmd + opt + i` and we should see the array of hedgehogs!

Now, we need to render them! Since this is an `array` of hedgehogs, we need to render each element, therefore we need to iterate over the array. Let's do this in a separate function to keep our code clean.

```js
const getHedgehogs = () => {
  $('#hedgehog-info').html('');

  fetch("https://hedgehog-party.herokuapp.com/api/v1/invites")  
    .then(response => response.json())
    .then(hedgehogs => renderInvitees(hedgehogs))
};

const renderInvitees = (hedgehogs) => {
  hedgehogs.forEach(hedgehog => {
    console.log(hedgehog);
  })
}
```

We can see in the console that we have each one! Now, let's use some HTML to actually append this data to the div with the ID of `hedgehog-info`.

```js
const renderInvitees = (hedgehogs) => {
 hedgehogs.forEach(hedgehog => {
   $('#hedgehog-info').append(`
    <article class="invited-hedgehog">
      <p class="name">${hedgehog.name}</p>
      <p class="hoglet-number">${hedgehog.hoglets}</p>
      <p class="allergies">${hedgehog.allergies}</p>
      <button id="${hedgehog.id}" class="uninvite-btn" aria-label="Uninvite">
        uninvite
      </button>
    </article>
    `)
 })
}
```

Once we save and run this - we should see the hedgehogs from our database rendered to the DOM. 🙌 Great work!

### Invite another Hedgehog
Right now, if we type info into the form, click the 'Invite' button - nothing happens. As front end developers, it's our job to take information from this form and send it to the database. Then, the database should update our master list of invitees.

First, we need to tell the computer to listen for a `click` of the 'Invite' button. When that click happens, we want to call the `addNewHedgehog` function. Let's write an event listener.

```js
$('#invite-btn').on('click', addNewHedgehog);
```

Now, we need to write some code in that function so that it:
- Captures all of the information the user typed in
- Sends that information to the database

Inside of our function, we will use the jQuery `$` to access specific IDs - in this case, `name`, `hoglets`, and `allergies`. We must include `.val()` at the end of each statement to access the value that was typed in, not just the HTML element itself.

```js
const addNewHedgehog = () => {
  var name = $('#name').val()
  var numHoglets = $('#hoglets').val()
  var allergies = $('#allergies').val()
};
```
Cool, we have the user input. We aren't sending it anywhere yet... Let's break that out into a separate function so we can stay organized.

```js
const addNewHedgehog = () => {
  var name = $('#name').val()
  var hoglets = $('#hoglets').val()
  var allergies = $('#allergies').val()
  postHedgehogs({name, hoglets, allergies})
};

const postHedgehogs = (hedgehogData) => {
  // code will go here
}
```

The `fetch` function defaults to 'getting' data - but this time we want to 'post' data. This means we will have to pass it some configuration settings. Don't worry about the details of this for now, just copy and paste this piece. If you would like to research it more, feel free to do so later, and follow up in the channel if questions come up!

```js
const postHedgehogs = (hedgehogData) => {
  fetch(`https://hedgehog-party.herokuapp.com/api/v1/invites`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(hedgehogData)
  })
  .then(response => response.json())
  .then(getHedgehogs)
}
```

We should now see our new hedgehog appear - both at 'https://hedgehog-party.herokuapp.com/api/v1/invites' and in our browser! We completed 2 of our 3 tasks, keep it up!



### Uninviting a Hedgehog 🔥
The last piece of this app is making sure our party planner has the ability to un-invite a hedgehog/hedgehog family. We are given two pieces of code to start us off; let's make sure we understand what it's doing for us:

```js
const unInviteHedgehog = () => {

};

$('#invited-hedgehogs-info').on('click', '.uninvite-btn', unInviteHedgehog);
```

Above, we have a function called `unInviteHedgeHog` which currently isn't doing anything. Below, we have an click event listener on the button with a class of `uninvite-btn`. When that button is clicked, the `unInviteHedgeHog` function will be called.

Let's write some code in our function so that something actually happens when the uninvite button is clicked! We need to do two things here:
- Remove the row of the table from the DOM, so the user cannot see it
- Remove this hedgehogs data from the database

Let's start with the DOM:

```js
const unInviteHedgehog = (event) => {
  event.target.parentNode.remove()
};
```

We took note of where the event happened (button), traversed up to the parent (the entire row), and used a built-in JavaScript function that removes the HTML element it is called on.

Now, let's call a function that will delete this hedgehog from the database. We are passing this `deleteHedgehog` function the hedgehog ID which the database created for us, so needs to know the ID if it wants to change or delete it.

```js
const unInviteHedgehog = (event) => {
  event.target.parentNode.remove()
  deleteHedgehog(event.target.id);
};

const deleteHedgehog = (hedgieId) => {

}
```
Lastly, we need to make the delete request. We will use the `fetch` tool, but slightly differently than before:

- pass the ID into the URL, using string interpolation
- tell the computer we want to delete this

```js
const unInviteHedgehog = (event) => {
  event.target.parentNode.remove()
  deleteHedgehog(event.target.id);
};

const deleteHedgehog = (hedgieId) => {
  fetch(`https://hedgehog-party.herokuapp.com/api/v1/invites/${hedgieId}`, {
    method: 'DELETE'
  })
}
```

Our delete functionality should be working 🎉
