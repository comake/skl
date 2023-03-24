# Verbs

A Verb is a representation of a capability in one or more software tools.

Software tools often expose similar capabilities that vary slightly in their parameters, outputs, and execution methods, even though they may have the same meaning or eventual effect. Verbs create a simplified way through which developers can use the general capabilities of software tools without having to know the specific requirements or formats of each tool's API.

The schema for a Verb specifies its expected parameters and expected return value. You can see the schema for a Verb in the [SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/nouns/verb/schema.json).

## Example

Let's say a developer is building a productivity application which displays a user's files from [Dropbox](https://www.dropbox.com/), saved articles from [Medium](https://medium.com/), and tasks from [Asana](https://asana.com/). The developer could build into their application a single interface to "share" any of these files, articles, or tasks with a co-worker. When a user shares something, the developer's code can use the `share` Verb with the entity to be shared and a Person entity (who the entity is being shared with) as parameters. The relevant Mappings (defined below) can then be used to translate the parameters into a properly formatted request to send to the Dropbox, Medium, or Asana API depending on the type and source Integration of the entity being shared.

In this way, Verbs are intelligently executed according to the context in which they are used.

## Schedules

In addition to being explicitly called by a developer's code, Verbs may be configured to be run at a specific time, upon a specific schedule, or in response to specific events. For example, a Verb can specify that it should be run in response to an event from a webhook registered with the API of an Integration.
