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
