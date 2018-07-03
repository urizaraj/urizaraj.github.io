---
layout: post
title:      "Hosting a site"
date:       2018-07-03 02:10:07 +0000
permalink:  hosting_a_site
---


One goal that I have is to host a site, that other people can access. With my interest in full stack web development, it feels like the final frontier that I have left to explore.

To start, I did some research. Between sources such as Google and Reddit, I learned about a lot of different services that you can use to start a website. If you're looking to deploy an app using a Python framework, there's [Python Anywhere](https://www.pythonanywhere.com/) that looks dead simple to set up. There's [Heroku](https://www.heroku.com/home) where it looks like you can simply push code using git to deploy apps in a variety of different languages. There's even [Amazon Web Services](https://aws.amazon.com/). What I settled on, however, was [Digital Ocean](https://www.digitalocean.com/). While perhaps not as plug-and-play as some of the other options, I like challenge of having to better understand a piece of technology to get something to work. Plus, I heard Digital Ocean has lots of great tutorials, so I predicted I wouldn't be too lost in the woods.

After I signed up for an account, I needed to download some extra programs to actually connect and communicate with the server. [PuTTY](https://www.putty.org/) is the SSH client I'm using. In terms of transferring files to the server, I had a couple options. One popular option is to install git on the server, and pull a repository with everything I need right, for example a website posted right to GitHub. Right now, I don't have anything on GitHub that I would consider production ready just yet, but it's something I will keep in mind for the future. Otherwise, you can download an FTP client and transfer files directly. In these initial phases, I decided to download [WinSCP](https://winscp.net/eng/index.php) and transfer files that way. Right now, I'm more focused on getting things to work above creating a sustainable workflow.

I decided to follow [this tutorial](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-16-04) for hosting a Node.js application on a Digital Ocean droplet. There were two main takeaways from this tutorial - you can use PM2 to daemonize applications and you can use Nginx to set up a reverse proxy server. While this isn't fully functional right now, my thought is to have a mongoDB database listening on one port locally on the server, an express API application connected to the database and listening on another port, and a React application proxying the express application on a third port. Then, use NGinx to create a reverse proxy from a public IP address to the React application. Right now, I just have the reverse proxy set up, using [this tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-16-04) to get that set up. Next steps are to get the express server and mongoDB database set up.

Attempting to host a website through a service such as Digital Ocean can definitely be challenging, but it's definitely helping me understand how all the pieces of the web can fit together. Sometimes the best way to learn is to just jump in, so I will be sure to give updates with my progress and share any lessons.
