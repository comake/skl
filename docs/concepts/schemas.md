# Schemas

## What is a schema?

Many software tools and frameworks use schemas as a basis for describing the data formats used by and function of a system. For example, GraphQL uses schemas to define the data types and possible operations of an API. SQL servers use schemas to define the types and fields of objects in a database and informs what queries are possible.

## How are Schemas used in SKL? 

In SKL, schemas are used to describe everything: Integrations, Accounts, APIs, Nouns, Verbs, Mappings, etc. They aren't tied to any specific database or storage engine and 
are written declaratively, making it so that they can be used in any programming environment. This enables them to be re-used by many developers and projects, including as a cross-language type system. 

SKL Schemas are modeled using the Resouce Description Framework ([RDF](https://en.wikipedia.org/wiki/Resource_Description_Framework)), and are most commonly viewed and edited in the [JSON-LD](https://json-ld.org/) format. As RDF, Schemas are identified by and relate to one another via URI. This forms a graph of the data formats and capabilities a project uses.

When building software with SKL, Schemas serve three main purposes:

1. To describe and document data structures
2. To validate that data conforms to specific data structures
3. As configuration detailing what capabilities are possible and how those capabilities are performed

SKL Schemas make it so that:

* Data can be understood by what it represents rather than relying on where it comes from or where it is stored. This is done using Nouns.
* A developer, end user, or application can easily discover what Verbs (i.e. software capabilities) can be used over any given piece of data (a Noun) and vice versa.
* A developer, end user, or application can perform operations on/with any given Noun(s) regardless of where the data comes from or how it’s stored.
* Complex workflows, applications, and automations which combine data from multiple Integrations can be easily built and customized with Nouns and Verbs such that you don’t have to interact with tool specific APIs nor create and manage specialized code for each.

