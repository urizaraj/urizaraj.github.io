---
layout: post
title:      "Choose Your Own Adventure"
date:       2018-04-12 17:18:12 -0400
permalink:  choose_your_own_adventure
---

*A Rails App with a jQuery Front End project*

For my Rails App with jQuery Front End project, I created a website where users can read stories with branching paths, create their own stories, and add new branches to existing stories.

![View a Story](https://imgur.com/3cNWua0)

## Models

This project consists of three models - `User`, `Story`, and `Branch`. The implementation of these models is interesting - a story has a starting branch **and** many branches, while each branch has a parent branch and many child branches. I implemented that relationship on the models like this:

```
class Story < ApplicationRecord
  belongs_to :start_branch, class_name: 'Branch'
  belongs_to :user
  has_many :branches
	
	accepts_nested_attributes_for :start_branch
	...
end
```

```
class Branch < ApplicationRecord
  belongs_to :parent, class_name: 'Branch', optional: true
  has_many :branches, foreign_key: 'parent_id'
  belongs_to :story
  belongs_to :user
	...
end
```

Stories belong to a start branch by adding the `class_name: 'Branch'` option to diferentiate between starting branches and other branches. Meanwhile, on braches belong to a parent. I added `optional: true` because a starting branch does not have a parent. If this were left out, you would not be able to create a starting branch along with a new story, as adding a belongs_to relationship to a model requires that relationship be present before commiting the transaction to the database.

The main limitation of this approach is that, currently, a branch can only have one parent - creating stories where two options lead to the same result is not possible. 

## Viewing a Story

Viewing stories is the main way that users interact with the site, so I wanted this to be a seemless and intuitive experience.

When the user first visits the page, the story, the starting branch, and a form to create a new branch is rendered. Behind the scenes is where things get interesting. In the Javascript world, I created two classes: an Application class and a Branch class. When the page first loads, a new Application instance is initialized. When initialized, a new instance of the Branch class is set to the applications's startBranch attribute, representing the starting branch.
Each instance of the branch class has a 'load' method that handles the AJAX - when you load a branch, it updates itself with the JSON data received from the server, creates new instances of the Branch class for its child branches, and displays itself and its child branches on the page. 

This allows you to read an entire path of a story without reloading the page - you click on a child branch, the child branch is loaded to the page, and you can select the next branch. Some branches can be endings - in these cases, a 'start over' button is displayed on the page, which loads the starting branch on the page. Other branches are returnable, which displays a 'go back' button on the page which loads the parent branch.

You can also seamlessly add new branches - a 'new branch' form is loaded as a part of the view. As you navigate through branches, instances of the branch model will update the 'branch_parent_id' field of the form to reflect what the current parent branch will be for a new branch. Javascript intercepts the default action for submitting the form and instead uses AJAX to submit the form to the server asynchronously. The response is then used to create a new Branch instance and its link is added to the page. You can see the code for this here:

```
    submitForm(event) {
      event.preventDefault()
      const values = form.serialize()
      const resp = $.post(`/stories/${sid}/branches`, values)
      resp.done(data => {
        const branch = new Branch(data, this.curBranch)
        branch.addLink()
        form[0].reset()
      })
    }
```
		
Variables such as `form` and `sid` are constants that I declared beforehand using a closure - I defined my classes within a function and declared constants within that same function so that instances of my classes would be able to reference those variables. I used this technique to create constants for jQuery objects that I know could be used multiple times.

## Final Thoughts

This was an interesting project to tackle. Using both Ruby and Javascript to perform similar actions has its challenges - in order to keep my project DRY, I had to be thoughtful about Javascript and Ruby overlapping as refactoring was no longer a case of just creating a new partial view or helper method. Still, with Rails Active Record Serializers and the Asset Pipeline, things were able to come together well.
