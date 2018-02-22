---
layout: post
title:      "AV Club Parser"
date:       2018-02-22 22:05:19 +0000
permalink:  av_club_parser
---


[Link to this project on GitHub](https://github.com/urizaraj/av-club-parse)

## Introduction

In this project, I designed a web scraper to parse [the AV Club newswire](https://www.avclub.com/c/newswire), my favorite source of pop culture news. This project is composed of 5 classes - `AVParseCommandLine`, `AVParser`, `Scraper`, `Article`, and `Tag`. Together, you can browse the latest articles, read individual articles, fetch more articles, and browse by article tags.

### `AVParseCommandLine`

This class is used to interact with the user on the command line. Upon starting, 20 articles are loaded and the user is prompted to enter a command. I created a hash called `COMMANDS` where the keys are potential inputs and the values are methods to execute those commands. After the user inpts a command, the string is converted to a symbol - if the hash has that key, the corresponding value is executed. The instance variable `@arg` stores a potential argument if a method needs one. 

### `AVParser`

This class is used to interact with the `Scraper`, `Article`, and `Tag` classes. This class keeps track of all the created articles and tags. Generally, the `AVParser` class will interact with the `Scraper` class to obtain general information and create articles and tags with that information.

With the AV Club newswire, there are no 'pages' - in the URL a start time is specified in Epoch time and the next 20 articles past that are displayed. To include functionality to retreive more articles, the `AVParser` class will retrieve the publish date of the last article, convert it to Epoch time, and append that to the URL.

### `Scraper`

This class scrapes index pages and article pages. This class can stand alone - it does not need to have an awareness of any of the other classes created in this project.

### `Article`

Each Newswire article is an instance of the `Article` class. When an instance is created, only the name, summary, url, and date need to be provided. Within the `AVParser` class is a method to update an article with its full text and associated tags. Each article has an ID instance variable that corresponds to its index in the list of all articles in the `AVParser` class, but this ID is not needed when initialized nor is it used by any of its methods.

### `Tag`

Each Newsire article is associated with a list of tags, and each tag is an instance of the `Tag` class in this project. Oher than a name and URL, the tag class also maintains a list of Article instances. 

## Final Thoughts

A goal was to keep this project modular - a CLI was created, but I wanted to design the `AVParser` class in a way where it could be used in other applications. I enjoy reading the AV club, so I have some possible ideas of where I would want to take this code next.
