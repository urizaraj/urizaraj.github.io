---
layout: post
title:      "Express, Typescript and async / await"
date:       2018-06-26 03:28:02 +0000
permalink:  express_typescript_and_async_await
---


On the support desk website that I am designing, I most recently switched from Sails to Express. There were no strong reasons why - I wanted to get more familiar with Express and MongoDB and decided that using a more minimal framework would force me to better understand how it works.

With the switch to Express, I had a couple goals in mind - take advantage of Typescript, and take advantage of async / await. After digging in, implementing these two goals were really easy.

There's a helpful [quick start guide](https://github.com/Microsoft/TypeScript-Node-Starter#typescript-node-starter) for getting up and running with Typescript and Express from the ground up. For the purposes of my application, utilizing Express exclusively as an API, the starter code does have some unneeded files (like the various Pug views) but there was still a lot that could be used. 

Here's an example Mongoose schema that I created:

```
import mongoose from 'mongoose'

export type TeamModel = mongoose.Document & {
  name: string
}

export const teamSchema = new mongoose.Schema(
  {
    name: String
  },
  {
    timestamps: true,
    toJSON: { getters: true }
  }
)

const Team = mongoose.model<TeamModel>('Team', teamSchema)
export default Team
```

This was the general pattern that I used to implement my models - create a type outlining the attributes of each document and export a model with the type passed in. With this pattern, any time the Team model is imported into another module, Typescript will make sure that I am only accessing attributes that actually exist on the model in the appropriate way. It's a great way to prevent bugs.

Here's an example controller action:

```
import { RequestHandler } from 'express'
import Ticket from '../models/Ticket'

export const getTickets: RequestHandler = async (req, res) => {
  const tickets = await Ticket.find({}).populate('team')

  res.json(tickets.map(ticket => ticket.toJSON()))
}
```

In a lot of Express and Mongoose code I saw online, there were many callbacks that, while fully functional, were not the prettiest to look at. This pattern avoids that. For starters, any route action will have the type of `RequestHandler` to ensure that `req` and `res` are used properly. Also, by specifying that this action is an async function, we can use await - this allows us to sidestep callback functions and write (in my opinion) much clearer code. Finally, as this route action will be used for the API I am creating, I return a JSON response. 

In these short couple of examples, you can see a glimpse of some of the advantages of using Typescript and async / await. Typescript will throw errors if you write your functions incorrectly and async / await can lead to clearer code. As I dig further into my application, I will be sure to post more that I learn.
