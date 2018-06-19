---
layout: post
title:      "Sails, Blueprints, and Assocations"
date:       2018-06-19 03:45:02 +0000
permalink:  sails_blueprints_and_assocations
---


One thing that I've been tinkering around with is the [Sails framework](https://sailsjs.com/), inspired by Rails and written in Javascript. I was curious to work with a framework that also implements an ORM - with Sails, its called Waterline. One piece of functionality built in Sails are blueprints - when you create a model, blueprint routes are generated automatically that allow you to perform CRUD actions without any extra code.

In my 'Ticket' model, for example, I have some attributes defined:

```
module.exports = {
  attributes: {
    title: {
      type: 'string',
      required: true
    },

    category: {
      type: 'string',
      required: true
    },

    status: {
      type: 'string',
      defaultsTo: 'open',
      isNotEmptyString: true,
      isIn: ['open', 'onHold', 'closed']
    },

    priority: {
      type: 'number',
      defaultsTo: 3
    },

    content: {
      type: 'string'
    },

    posts: {
      collection: 'post',
      via: 'ticket'
    },

    user: {
      model: 'user',
      required: true
    },

    team: {
      model: 'team',
      required: true
    }
  }
};
```

Without even needing to configure a controller to determine what to do with a POST request, I can create new tickets automatically. In react, for example, I can implement the following code:

```
const state = getState()
const ticket = {
  ...state.ticket.form,
  user: state.account.id
}
const options = {
  method: 'POST',
  body: JSON.stringify(ticket)
}

return fetch('/ticket', options)
  .then(checkResp)
  .then(resp => history.push(`/tickets/${resp.id}`))
  .catch(err => console.log('error', err))
```

I'm already using camelCase for attributes in my React front-end, so I don't need to make any adjustments there. As long as I stringify the state, I can send a post request that contains all the required attributes and the server will know what to do automatically. For relationships, you just need to specify the ID - sails will do the rest.

If you need to overwrite the base sails behavior, you can still do that, but otherwise for rapidly developing a web app, the brevity that blueprints provide make for an enjoyable experience.
