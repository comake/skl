THIS IS OLD AND NOT USED

# Verbs

A Verb is a representation of a capability in one or more software tools.

Software tools often expose similar capabilities that vary slightly in their parameters, outputs, and execution methods, even though they may have the same meaning or eventual effect. Verbs create a simplified way through which developers can use the general capabilities of software tools without having to know the specific requirements or formats of each tool's API.

## Example

Let's say a developer is building a productivity application which displays a user's files from [Dropbox](https://www.dropbox.com/), saved articles from [Medium](https://medium.com/), and tasks from [Asana](https://asana.com/). The developer wants to add the ability for users to "share" any of these files, articles, or tasks in the application. When a user shares something, the developer's code can use the `share` Verb with parameters representing the entity to be shared and a Person entity (who the entity is being shared with) as parameters. The relevant [Mappings](./mappings.md) can then be used to translate the parameters into a properly formatted request to send to the Dropbox, Medium, or Asana API depending on the type and source [Integration](../../../spec/introduction.md#integration) of the entity being shared.

In this way, Verbs are intelligently executed according to the context in which they are used.

## Verb Schema

The schema for a Verb specifies its expected parameters and expected return value:

| name | Type | Required | Description | Cardinality |
| ---- | ---- | ---- | ----------- | ---- |
| parameters | [NodeShape](http://www.w3.org/ns/shacl#NodeShape) | false | A SHACL NodeShape specifying the format and constraints that the parameters of a Verb must conform to. | 0..1 |
| parametersContext | [JSON](http://www.w3.org/1999/02/22-rdf-syntax-ns#JSON) | false | A JSON-LD Context Definition used to expand the parameters supplied a Verb so that they can be validated against the Verb's `parameters` NodeShape. | 0..1 |
| returnValue | [NodeShape](http://www.w3.org/ns/shacl#NodeShape) | false | A SHACL NodeShape specifying the format and constraints that the return value of a Verb must conform to. | 0..1 |
| returnValueFrame | [JSON](http://www.w3.org/1999/02/22-rdf-syntax-ns#JSON) | false | A JSON-LD Frame used to transform the JSON-LD returned by a Mapping to this Verb. | 0..1 |

You can see the full schema for a Verb in the [SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/schemas/core/verb/schema.json).

## Schedules

{% hint style="warning" %}
🚧 Under development. In addition to being explicitly called by a developer's code, we envision Verbs being able to be configured to be run at a specific time, upon a specific schedule, or in response to specific events. For example, a Verb can specify that it should be run in response to an event from a webhook registered with the API of an Integration. Verb scheduling will likely use a format like [cron](https://en.wikipedia.org/wiki/Cron). 

Please see the [Issue on Github](https://github.com/comake/skl/issues/1) or [contact us](https://discord.gg/stvfSB8kpG?ref=https://github.com/comake/skl-examples) to get involved.
{% endhint %}

