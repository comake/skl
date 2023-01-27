Standard Knowledge Language defines abstractions of data and capabilities through schemas.

## What is a schema?

**Schemas** are sets of declarative configuration that describe structured data and software components and how the data formats and capabilities of those software components can be used. Many software tools and frameworks use schemas as a basis for describing the function of a system. For example:
- GraphQL uses schemas to define the data types and possible operations of an API
- SQL servers use schemas to define the types and fields of objects in a database and informs what queries are possible

### Standard Knowledge Schemas

When building software with SKL, schemas are used for three main purposes:
1. to describe and document data structures
2. to validate that data conforms to specific data structures
3. as configuration detailing what capabilities are possible and how those capabilities are performed

SKL schemas can be one of three types:
- **Nouns**: describe data structures representing concepts commonly used by software tools
- **Verbs**: represent capabilities exposed by software tools (and their parameters and return values)
- **Mappings**: sets of configurations which dictate how a program can translate between Nouns and Verbs and the unique API of any software tool.

The Nouns, Verbs and Mappings of SKL make it so that:
- Data can be understood by what it represents rather than relying on where it comes from or where it is stored. This is done using Nouns.
- A developer, end user, or application can perform operations on/with any given Noun(s) regardless of where the data comes from or how it’s stored.
- A developer, end user, or application can easily discover what Verbs (i.e. software capabilities) can be used over any given piece of data (a Noun) and vice versa.
- Complex workflows, applications, and automations which integrate data from multiple tools (i.e., Integrations) can be easily built and customized with Nouns and Verbs such that they don’t have to interact with tool specific APIs nor create and manage specialized code for each.

## Nouns {#nouns}
A Noun is a schema for a data structure representing a concept used by software tools. When you use SKL, everything is translated through a Noun: emails, notes, webpages, files, podcast episodes, news articles, receipts, prescriptions, medications, locations, etc.

Many software tools use data structures with the same name but with slight differences. Nouns are an abstraction of these different data structures that enables a higher degree of interoperability between them.

##### Example

The developers of [Google Drive](https://www.google.com/drive/), [Dropbox](https://www.dropbox.com/), and [Box](https://www.box.com/) each defined a different data structure used for representing files in their respective system. Normally, when integrating files from these three tools into an application, a developer would have to write different code to work with each. Instead, using SKL, the unique data structures of each tool can be translated (see [Mappings](/fundamentals#mappings) for info about the translation process) into instances of the File Noun. The resulting File entities from each system include common fields like name, mime type, and size. They may also include fields that only some systems support such as a unique hash of the contents of the file, which Box and Dropbox support but Google Drive doesn't. Dropbox also has support for adding any arbitrary key/value data to files, this information will be included in any File entities from Dropbox.

In this way, Nouns are abstractions of the commonalities between each Integration’s representation of a concept but are also extendable and customizable, allowing the unique fields each tool has to still exist on entities.

##### Entity {#entity}
An entity is an instance of a Noun which conforms to the Noun's schema.

##### Properties
A Property of a Noun is a characteristic that has a label and a value, such as the `title` property of a File, or the `status` property of a Task. Each property has a schema that can defines its name, description, type, default value, range, cardinality, length or pattern. Properties of nouns can be required (having a minimim cardinality of one).

##### SHACL
The schema that entities of a Noun must conform to is defined using the [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/). You can see an example of the File noun as a SHACL shape in the [SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/nouns/file/schema.json). 

{% hint style="success" %} 
Most Noun schemas have the `shacl:closed` property set to `false`. This means that data conforming to the schema can include any arbitrary properties not explicitly enumerated as a property in the schema. 
{% endhint %}

{% hint style="info" %} We acknowledge that it's impossible to create a single ontology that can appease every use case. In addition, it's a huge undertaking to attempt to model an abstraction for every data structure a software tool may need to work with. In light of these facts, SKL Schemas are highly modular, customizable, and are evaluated at runtime to enable any capabilities an application or end user may need. In addition, we are continuously adding to the [SKL Dictionary](https://github.com/comake/skl-dictionary),  an open source Library of Nouns, Verbs, and Mappings which developers can use and contribute to. {% endhint %}

## Verbs {#verbs}

A Verb is a representation of a capability in one or more software tools.

Software tools often expose similar capabilities that vary slightly in their parameters, outputs, and execution methods, even though they may have the same meaning or eventual effect. Verbs create a simplified way through which developers can use the general capabilities of software tools without having to know the specific requirements or formats of each tool's API.

The schema for a Verb specifies its expected parameters and expected return value. You can see the schema for a Verb in the [SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/nouns/verb/schema.json).

##### Example 
Let's say a developer is building a productivity application which displays a user's files from [Dropbox](https://www.dropbox.com/), saved articles from [Medium](https://medium.com/), and tasks from [Asana](https://asana.com/). The developer could build into their application a single interface to "share" any of these files, articles, or tasks with a co-worker. When a user shares something, the developer's code can use the `share` Verb with the entity to be shared and a Person entity (who the entity is being shared with) as parameters. The relevant Mappings (defined below) can then be used to translate the parameters into a properly formatted request to send to the Dropbox, Medium, or Asana API depending on the type and source Integration of the entity being shared.

In this way, Verbs are intelligently executed according to the context in which they are used.

##### Schedules
In addition to being explicitly called by a developer's code, Verbs may be configured to be run at a specific time, upon a specific schedule, or in response to specific events. For example, a Verb can specify that it should be run in response to an event from a webhook registered with the API of an Integration.

## Mappings {#mappings}

A Mapping is a set of configuration which dictates how a program can translate between Nouns and Verbs and the unique API of any software tool.

Mappings are the glue which holds the components of SKL together and that allow them to interoperate. By having Mappings defined separately from Nouns, Verbs, and Integrations, users or developers can more easily compose components of the system together.

The schema for a Mapping has a name, a type, and sets of declarative rules that define how Nouns or Verbs are to be transformed. Mappings use the [RDF Mapping Language (RML)](https://rml.io/) to express how data is translated.

SKL includes multiple types of Mappings for translating between different types of components.

##### Data-Noun Mapping
A Data-Noun Mapping either translates an entity conforming to the schema of a Noun into a unique data structure (eg. to be used as the parameters of an Integration’s API), or translates a unique data structure (eg. returned from an Integration’s API) into the schema of a Noun. Data-Noun Mappings are often used by reference by Verb-Integration Mappings to avoid duplication of Mappings.

##### Verb-Integration Mapping
A Verb-Integration Mapping translates the parameters of a Verb to the unique parameters and API endpoint of an Integration to execute and perform the intent of the Verb using the Integration. Verb-Integration Mappings also include a conversion of the response of the API endpoint to the standard return value of the Verb. They may reference one or more Data-Noun Mappings.

##### Noun-Interface Mapping - 🚧 Under development
A Noun-Interface Mapping is a type of Data-Noun Mapping translates a Noun into the inputs of an interface component.

##### Example

You can see an example of the Verb-Integration Mapping which translates between the [`getFilesInFolder`](https://github.com/comake/skl-dictionary/blob/main/verbs/getFilesInFolder/schema.json) Verb and the [Dropbox](https://github.com/comake/skl-dictionary/blob/main/integrations/dropbox/schema.json) Integration [here in the SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/mappings/getFilesInFolderToDropbox/schema.json).