---
layout: post
title:      "Order Online"
date:       2018-05-09 20:53:47 +0000
permalink:  order_online
---

A React project

In this project, I designed an interface for ordering food online. My experiences with ordering food online in the past has been subpar, so I wanted to see how using a front-end framework like React with a Rails backend could be an improvement.

Check out the repository [here](https://github.com/urizaraj/order-online).

## Designing the API

From the start, I knew this project would have two main models - locations and orders. I designed locations to be flexible - each location has a menu, each menu has many categories, each category has many items, and each item has many options. Orders parallel locations - an order has many order items, and an order item has many selected options. With this design, the database can persist many different kinds of locations and heavily customized orders.

## Creating and Editing Locations

With each location having so many associations, I designed the form for creating and editing a location to allow creating and editing associations all at once - needing to go into a separate form to create a category, for example, seemed cumbersome and unnecessary.

The state of the form is maintained in redux - the state includes attributes for the location and empty arrays for categories, items, and options. The form starts off with no fields for a category, item, or option - a user can instead click a button to add a resource dynamically, with each resource tracked with a cuid. This way, the form doesn't need to know how many resources are going to be created ahead of time. In order to preserve associations, each item stores its category cuid and each option stores its item cuid. On submission, items and options are grouped by their parent cuid so that I could take advantage of models accepting nested attributes for associations.

I was able to reuse the same form for creating and editing locations by pulling information from rails in the same way that it would be stored in redux and creating different actions for sending a patch request to the server.

## Creating New Orders

The form for creating a new order was interesting to design - for the most part, the user will be interacting with location resources, so the form for a new order consisted almost entirely of existing record fields. I took the same approach as locations to orders for maintaining state - each field is maintained in redux. 

One challenge was the presentation of the form - it would be overwhelming to the user to see every item and every option they could select, all at once, so the application also maintains an active item that a user is customizing to know which item to display all the details for. The application also maintains in state whether a user is in the check out phase of the order to conditionally display fields for payment type, address, tip, etc. 

## Lessons Learned

There were a few key lessons that I learned from this project.

First, refactoring was more time consuming than expected, so doing things right the first time is important. I kept my code modular and tried to reuse components where it made sense, but it made refactoring difficult. I used diagrams to keep track of how my components were connected so ensure that when I updated something in one location, I could make sure that I would be updating all the affected components appropriately. React is not as opinionated as Rails, so even things like naming conventions lead to a lot of refactoring that took time.

Second, keep state as closely aligned with models as possible. When I started coding my forms, I focused more on getting the functionality down first instead of making sure state was maintained in the best way. While this lead to faster prototyping, every shortcut I took in the front end ended up being another line of code needed on the backend to manipulate the data into the correct format, which got cumbersome. 

## Final Thoughts

I view this project as a success. I know for sure that it will be tough to go back to ordering food online from less elegant websites. 
