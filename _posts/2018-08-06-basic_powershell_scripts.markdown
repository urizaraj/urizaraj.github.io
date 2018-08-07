---
layout: post
title:      "Basic PowerShell Scripts"
date:       2018-08-07 02:58:26 +0000
permalink:  basic_powershell_scripts
---


One thing that I have been playing around with is PowerShell. PowerShell, a task automation and configuration management framework from Microsoft, allows you to do some pretty powerful things. Plus, for those familiar with object oriented programming languages, it allows you to interact with your computer using .NET objects, which allows you to interact with your computer in an intuitive way. There's a lot you can do, and [there are many sample scripts in the documentation](https://docs.microsoft.com/en-us/powershell/scripting/getting-started/fundamental/using-windows-powershell-for-administration?view=powershell-6) but here are some things that I have been playing around with lately.

First, if you are going to be typing powershell scripts from a command prompt, you might want to save yourself some keystrokes by learning aliases for more common commands. For example, `dir` is an alias for `Get-ChildItem`. To get an understanding of how piping works with PowerShell, here's a command to get a list of all the current aliases in effect:

`gal | ft name, resolvedcommandname`

`gal` is an alias for `Get-Alias` and `ft` formats the output in a table, showing the name and resolved command of all aliases. We can take this a step further and instead convert the output to JSON and save to a file like this:

`gal | select name, resolvedcommandname | ConvertTo-Json | Out-File alias.json`

`ConvertTo-Json` is a useful cmdlet that allows you to convert output to JSON - it feels like all programming languages have an intuitive way to deal with JSON, so this is a great way to get output into a format that will work well anywhere. `Out-File` allows you to write output to a specified path. 

Finally, the king of all cmdlets: `Get-Command`. This will display all commands - it's an okay jumping off point for getting a sense of all the commands that are at your fingertips. There are lots of opportunities to use PowerShell, so I recommend checking out the documentation and maybe searching Google for things that you may want to do to get started.
