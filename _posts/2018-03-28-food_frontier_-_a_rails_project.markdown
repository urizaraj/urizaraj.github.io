---
layout: post
title:      "Food Frontier - A Rails Project"
date:       2018-03-28 16:44:18 +0000
permalink:  food_frontier_-_a_rails_project
---


For my Rails project, I designed a website where users can browse restaurants, read their menus, and write reviews. You can find the repository for this project [here](https://github.com/urizaraj/food-frontier).

## Process
### Models

Even a small app like this required a lot of models. 

- Restaurant
- MenuItem, each restaurant has their own menu items
- Item, such as a hamburger, or soda
- ItemTag, such as drink, entree, or side
- User
- Review

Additionally, I created an ItemToItemTag model, as each item could have many item tags.

The links between these models is logical - a restaurant has many menu items, each menu item belongs to an item, and an item can have many item tags. I added a validation to the Item model to ensure that it has a 'main tag' - it is labeled either a drink, side, or entree. Users have many reviews and can only have one review per restaurant. 

I used the devise gem for user authentication, and utilized its omniauth funtionality to allow users to sign in through Github. 

One interesting thing I did was add a 'rating' column to the restaurants table - when browsing restaurants, I wanted to be able to see the average rating each restaurant's reviews. On the Restaurant model, I added an 'update rating' method that averages all of the review ratings and updates the restaurant:

```
def update_rating
 ratings = reviews.pluck(:rating)
 value = ratings.empty? ? 0 : (ratings.reduce(:+).to_f / ratings.size)
 update(rating: value)
end
```

Then, on the Review model, I added 'after_save' and 'after_destroy' callbacks to update the restaurant's rating:

```
after_save :update_restaurant_rating
after_destroy :update_restaurant_rating

def update_restaurant_rating
 restaurant.update_rating
end
```
### Views
Standard CRUD actions here - you can view restaurants, items, and item tags. For restaurants, I created an additional view to allow an admin user to set all the prices of menu items for a restaurant at once, rather than having to edit each menu item individually. If I were to increase the scope of this project, I would consider adding additional fields to this form to allow an admin user edit the name of each menu item, add a custom description, and set the order of items. 
### Routes
Restaurants, items, item tags, and users have their own routes. I utilized nested routes for menu items and reviews - each menu item is nested under its respective restaurant and reviews are nested under their user or restaurant.
### Controllers
Reviews are nested under both users and restaurants, so I wanted to configure the reviews controller to handle setting the parent gracefully. I created a 'set parent' method that works like this:

```ruby
def set_parent
    restaurant = Restaurant.find_by(id: params[:restaurant_id])
    user = User.find_by(id: params[:user_id])
    @parent = restaurant || user
end
```

If the restaurant ID is in the path, the parent will get set to the restaurant instance. Otherwise, the restaurant variable will be nill, so the parent will be set to the user instead. There aren't routes to view individual reviews (viewing an individual review would be unnecessary) so I wasn't concered about a user manually visiting a page such as foodfrontier.com/restaurants/1/reviews/2 even though review 2 is not for restaurant 1.

For user roles, I kept it simple and gave users a boolean attribute indicating whether they are an admin or not. In the controller, for actions involving creating, editing, or deleting records, I added a before action to check if the user is signed in and if so, the user is an admin. 
### Styles
The bootstrap gem makes customizing bootstrap really simple, which is what I did for this project. In the app/assets/stylesheets/application.scss file, I tweaked the theme colors, font, and a couple other things with just a few lines:

```css
$font-family-base: 'Hind', sans-serif;
$link-hover-decoration: none;
$enable-rounded: false;
$theme-colors: (
  'primary': #B8214E,
  'warning': #FF9000,
  'secondary': #FF0054,
  'pixie': #390099
);

@import "bootstrap";
```

### Final Thoughts
I can see why rails is considered magic - if you're building a project from the ground up with Rails in mind, the lines of code you can save in even a small app is staggering. I enjoyed working with a framework where, when I wanted to add a piece of functionality, I asked myself how I could do it, not if I would be able to do it.
