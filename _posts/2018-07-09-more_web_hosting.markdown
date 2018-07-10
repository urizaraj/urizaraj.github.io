---
layout: post
title:      "More Web Hosting"
date:       2018-07-10 03:15:03 +0000
permalink:  more_web_hosting
---


Last week I started working on hosting an actual website. Before tackling a website that implements a web framework like Rails or Express and a database, I decided to try implementing a simpler site, like a recreation of my resume in a pleasing format. You can check out the result on my [personal website](urizaraj.com).

One thing that I did was purchase another domain name, so that a person would not need to type in an IP address to actually access the site. I used [this tutorial](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars) to get it set up. If you understand how DNS works, the process makes sense - by using Digital Ocean's DNS servers, Digital Ocean can map droplets they host to domain names associated with their servers. If you don't understand how DNS works, [it's a google search away](https://www.cloudflare.com/learning/dns/what-is-dns/).

For the actual content of the site, I decided to use React and eventually plan to serve it with a static server. For the CSS, I used [Bulma](https://bulma.io/). I was drawn to this Framework because it's open source and doesn't have any JavaScript dependencies, something that could make working with React potentially easier. In terms of layout, Bulma is strikingly similar to Bootstrap - you'll find yourself using a lot of columns. Bulma has less helper CSS utilities for tweaking things, which is an interesting difference - to get the layout you want with Bulma, you will either need to understand how to use all of Bulma's elements correctly or be fearless and write some custom CSS yourself, which is always a good skill to have. Either way, out of the box Bulma is capable of helping you create some beautiful web pages.

One interesting React component that I used in my site was [react collapse](https://github.com/nkbt/react-collapse), a component built off of the animation library react-motion. One thing that I like about jQuery is the slideUp and slideDown animations, so a react-motion implementation that allows for a prettier, physics-based approach appeals to me. 

Finally, I used the npm package [serve](https://www.npmjs.com/package/serve) on the server to serve my React app. I built the app for production, transferred the files to the server using WinSCP, and started the sever on port 3000 using the command `serve -p 3000` in the directory with all my files. The reverse proxy was already set up to direct traffic to that port, so it worked seamlessly. 

This has been an interesting exercise, and I am excited to dig into more.
