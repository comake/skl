# Verbs

## What is a Verb? 

A Verb is a representation of a capability exposed by the API of one or more Integrations. For example, Share, Send, Download, Like, etc.

## Why do we need Verbs?

Software tools often expose similar capabilities that vary slightly in their parameters, outputs, and execution methods, even though they may have the same meaning or eventual effect. Verbs create a simplified way through which developers can use the general capabilities of software tools without having to know the specific requirements or formats of each tool's API.

## Conceptual Example

Let's say a developer is building a productivity application which displays a user's files from [Dropbox](https://www.dropbox.com/), saved articles from [Medium](https://medium.com/), and tasks from [Asana](https://asana.com/). The developer wants to add the ability for users to "share" any of these files, articles, or tasks in the application. When a user shares something, the developer's code can use the `share` Verb with parameters representing the entity to be shared and a Person entity (who the entity is being shared with) as parameters. The relevant [Mappings](./mappings.md) can then be used to translate the parameters into a properly formatted request to send to the Dropbox, Medium, or Asana API depending on the type and source [Integration](./integrations.md) of the entity being shared.

In this way, Verbs are intelligently executed according to the context in which they are used.

