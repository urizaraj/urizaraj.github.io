---
layout: post
title:      "Sinatra Project - TV Show Organizer"
date:       2018-03-10 17:03:44 +0000
permalink:  sinatra_project_-_tv_show_organizer
---


For the Sinatra portfolio project, I created a web application where users can keep track of shows they would like to watch and have already watched. You can find the respository for this project [here.](https://github.com/urizaraj/tv-show-organizer)

## Overview

From the homepage, you are prompted to log in or sign up. You can also see a list of the most popular shows, and click on  each box to get more details about the show. In the top toolbar, there are also links to the users and shows pages, which will display all users and shows, respectively. 

![Homepage](https://i.imgur.com/oIqFPqe.png)

Clicking Sign Up brings you to the sign up page. If you try to sign up with an invalid username, a taken username, or leave any fields blank you are brought back to the sign up page and any validation errors are displayed.

![Sign Up](https://i.imgur.com/TNtxlL1.png)

Likewise, clicking Log In brings you to the log in page. Here, only username and password are required, and inputting any incorrect values also displays an error.

![Log In](https://i.imgur.com/cTxkzWW.png)

Once logged in, you can manage the shows associated with your account by clicking "Manage Shows" in the top toolbar.

![Manage Shows](https://i.imgur.com/FF4N2c4.png)

If you update your shows, you are redirected to your user page where you can view all your shows, with your watched shows placed at the bottom.

![User Page](https://i.imgur.com/jMfJ4F7.png)

Clicking on a show brings you to the detail view of the show, where you can see the show name, year, and a description if provided. If logged in, a toolbar displays below the description that lets you mark a show as watched/unwatched, add or remove a show from your account, edit the show, or delete the show. Finally, you will be able to see a list of all the users who have added the show to their account.

![Imgur](https://i.imgur.com/fV3MuRM.png)

You can also create a new show by clicking "New Show" in the top toolbar. You are brought to a form where you can enter a name, year, and optionally a description. You can click the checkbox to set whether the show is automatically added to your account upon creating the show as well.

Finally, clicking Log Out in the top toolbar will log you out of your account. You cannot create or edit new shows if you are not logged in, but you can still browse existing shows and users.

## Under The Hood

The database is composed of three tables - `users`, `user_shows`, and `shows`. `users` keeps track of all the users, emails, and passwords. `user_shows` links each user to their list of shows, and also has a column to indicate whether the user has watched the show. `shows` keeps track of each show name, year, and description.

These tables correspond to three models - `User`, `Show`, and `UserShow`. A `User` has many `user_shows` and `shows`, a `UserShow` belongs to a `user` and `show`, and a `Show` has many `user_shows` and `users`. The `User` model has a secure password using bcrypt and has methods for returning and finding users by a slug. Additional validations are specified in the `User` and `Show` models for validating input. 

There are three application controllers: `ApplicationController`, `ShowController`, and `UserController`. Their roles are predictable - `ApplicationController` contains the route for the index page and has helper methods for returning the current user, determining whether you are logged in, and determining if the current user has watched a specific show. `UserController` controls logging in, logging out, managing your show list, and displaying the user list and user detail pages. `ShowController` controls creating, editing, and deleting shows, adding or removing shows from a user's acount from the show detail page, and displaying the show list and detail pages.

## Style and Layouts

I created a themed version of Bootstrap for the main CSS stylesheet and used FontAwesome icons for a few buttons. View templates are rendered in Haml.

## Final Thoughts

I always am looking for new TV shows to watch, so I wanted to create an application that at least had the basics of my discovery process. I wanted to include functionality to not only browse all the shows in the database but also see what shows are popular and what individual users have added and watched. If I were to develop this further, I would definitely consider adding a Genre model to allow shows to have many genres and a Review model to allow users to create reviews for shows that they have watched.
