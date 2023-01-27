# Quick Start

{% hint style="warning" %} This page is in progress. More documentation is coming soon! {% endhint %}

### 1. Install an Engine

To quickly get started using SKL, you can use the prebuilt [SKL Javascript Engine](https://github.com/comake/skql-js-engine) in a node.js project. Alternatively, you can [Build an Engine](./engines.md) in your language of choice.


Install via npm or yarn:

```shell
npm install @comake/skql-js-engine
yarn add @comake/skql-js-engine
```

### 2. Define Schemas

We will be posting more documentation on schemas soon. For now, you can view examples of schemas in use in the [examples folder of skql-js-engine](https://github.com/comake/skql-js-engine/tree/main/examples).

 The [folder-structure-sync](https://github.com/comake/skql-js-engine/tree/main/examples/folder-structure-sync) example is a node.js module that can recursively loop through the folder structure of any Integration that works with the `getFilesInFolder` Verb.

### 3. Write code

Once you have schemas defined for your domain, all you have to do is write code using the Verbs and Nouns in your schema to build your application logic.

<!-- TODO: explain how open api stuff works -->
