---
layout: post
title:      "Writing a console application in C#"
date:       2018-07-17 02:31:57 +0000
permalink:  writing_a_console_application_in_c
---


One language that I've wanted to learn is C#, so what better way to learn a language is there than jumping straight in and creating an application? Microsoft has a guide on creating a [REST application](https://docs.microsoft.com/en-us/dotnet/csharp/tutorials/console-webapiclient) on their website -- by the end, you will have created an application that you can run from the command line that lists repositories created by the Dotnet foundation. I wanted to take it a step further and create an application that you can use to read my blog. Here are the steps that I took.

First, I installed the .NET Core framework on my computer - .NET used to be Windows-only, but more recently Microsoft has released .NET Core that allows for running .NET applications on Mac and Linux too. Once installed, you can run `dotnet new` to create a new dotnet application. 

To create a command line application to read my blog, I knew it was going to need two basic functions. First, you will need to see a list of blog posts available, and second you will need to be able to select a post and read it. For the first goal, you can utilize the Github API to obtain contents of a repository - [here's the link](https://developer.github.com/v3/repos/contents/). In the repository for my blog, all the posts are contained in the `_posts` directory, so it was a matter of pointing the application to the right URL:

```
var streamTask = client.GetStreamAsync("https://api.github.com/repos/urizaraj/urizaraj.github.io/contents/_posts");
```

In my main `Program` class, I created a property for all of the blog posts like so:

```
private static readonly List<Post> posts = new List<Post>();
```

I created a `ProcessPosts` method to retrieve all of the blog posts using the Github API and added them to the list like this: 

```
posts.AddRange(ProcessPosts().Result);
```

Once the list of posts are retrieved, the application needs to display the available post so that the user can select one. One quirk about C# is that, when iterating over a list, there are not any built-in ways to retrieve the index at the same time, so I display the posts and their index like this: 

```
var i = 0;
foreach (var post in posts)
{
  var output = $"[{i++}] {post.Name}\n";
  Console.WriteLine(output);
}
```

Then, the user is sent into a loop, waiting for their input which should be an index of a post that they would like to read and displaying the article to the screen like this:

```
while (true)
{
  int input;
  var result = Int32.TryParse(Console.ReadLine(), out input);
  if (result)
  {
    var markdown = ProcessSinglePost(posts[input].DownloadUrl).Result;
    Console.WriteLine(markdown);

  }
  else break;
}
```

The loop is broken if the user does not enter an integer. Processing a single post is straight forward - the Github API automatically includes a URL to download any files, so it's easy to download and show the text like this: 

```
private static async Task<string> ProcessSinglePost(Uri url)
{
  var streamTask = client.GetStringAsync(url);

  return await streamTask;
}
```

You can see the final result of this application [here](https://github.com/urizaraj/blog-cli). It was interesting taking the fundamentals from creating a Ruby application to an application in a different language - while the strong typing of C# takes some getting used to, if you are programming the right way, you shouldn't run into too many problems.
