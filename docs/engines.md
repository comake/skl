After defining Schemas, an SKL Engine will allow you to execute Verbs and Mappings from your programming language of choice. An Engine can be built in any programming language that can read the Schema! 

## Concepts

In most programming languages and libraries, a function's implementation is defined ahead of time and has a predefined and specific purpose. This is conventional because it reduces ambiguity and allows code to be precise. However, because each language or library has potentially hundreds of function definitions, developers have to read lots of documentation and write code to use the unique functions of each language or library they need to incorporate.

SKL is different in that it uses Nouns and Verbs more like natural language. In natural language, words we use are sometimes understood contextually based on the other words they are paired with (eg. stirring a pot vs stirring in bed). When a developer calls a Verb using an Engine, the Engine uses the context the Verb was called in to dynamically determine what to do. The context includes what Mappings exist in the schema, and the source Integration and types of the Nouns that were used as arguments to the Verb.

For example, lets say a developer is building a productivity application which displays a user's files from [Dropbox](https://www.dropbox.com/), saved articles from [Medium](https://medium.com/), and tasks from [Asana](https://asana.com/). The developer could build into their application a single interface to "share" any of these files, articles, or tasks with a co-worker. When a user shares something through this interface, the developer's code can call the `share` Verb with a `Person` entity and the entity being shared (a `File`, `Article`, or `Task`) as parameters. The Engine should first search for the configuration of the `share` Verb in the schema to make sure the right parameters were supplied. Then, it will find a Mapping relating the `share` Verb, the source Integration of the entity being shared, and the type of the entity being shared.

In the case that an Asana task is being shared, the Mapping would translate the Person and Task parameters into a web request to the Asana API with the unique parameters, headers, and endpoint URL required. It sends the request then translates the API response into the standard `share` Verb response to return to the developer. This process happens automatically and completely within the Engine.

## Get Started

Now that you know what an SKL Engine does, you're ready to get started with one. Currently there are two choices:

- [Get started with the SKL Javascript Engine](./quick-start.md) in Node.js
- [Build an Engine](./build-an-engine.md) in your language of choice

We envision Engines being built in all major programming languages so that any project can use SKL, no matter the environment. If you're up to the task of creating an Engine in another language, or simply want to voice your desire for an Engine in a specific language, please [reach out to us on Discord](https://discord.gg/stvfSB8kpG?ref=https://github.com/comake/skl-examples) or [open or contribute to a discussion](https://github.com/comake/skl/discussions).

**Note**: SKL Engines used to be called Standard Knowledge Query Language (SKQL) Engines.
