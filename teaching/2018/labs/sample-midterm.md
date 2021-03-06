---
layout: assignment
title: Sample Midterm
date: 2018-09-21
---

## Client-side Templating

Download the [starter files](/teaching/2018/labs/midterm-starter.zip).

Make an Ajax request to [this endpoint](/teaching/2018/api/restaurants.json) and render the array of restaurants in the `data` attribute using client-side templating with Handlebars.js. Display all attributes of each restaurant. The `ownedBy` attribute should be rendered as an unordered list. The `yelpPage` attribute should be rendered as a link that opens in a new tab. The text of this link should say "Check us out on Yelp". If the restaurant accepts Apple Pay, display an Apple icon. Otherwise, display the text "No Apple Pay". While the response is loading, display "Loading...".

Push this code up to a repo named `itp404-lab2-part1`.

## Ember.js

Create a new Ember app and install [`ember-cli-mirage`](http://www.ember-cli-mirage.com/docs/v0.4.x/).

Build a mock REST API with Mirage for a `user` resource for the following operations:

* listing users
* viewing a single user's details

Use a Mirage factory and create randomized values for the following attributes using faker.js:

* `firstName`
* `lastName`
* `jobTitle`
* `phoneNumber`

Next, build out the functionality for listing all users and viewing more details about a user.

The URLs for each page should be:

* user list - /users
* user details - /users/:id

The user list page should only show `firstName` and `lastName`. The user detail page should show all attributes for its users.

Push this code up to a repo named `itp404-lab2-part2`.

Email both repo links to the TA and myself.
