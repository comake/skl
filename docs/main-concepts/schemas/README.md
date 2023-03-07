# Schemas

Standard Knowledge Language defines abstractions of data and capabilities through schemas.

## What is a schema?

**Schemas** are sets of declarative configuration that describe structured data and software components and how the data formats and capabilities of those software components can be used. Many software tools and frameworks use schemas as a basis for describing the function of a system. For example, GraphQL uses schemas to define the data types and possible operations of an API. SQL servers use schemas to define the types and fields of objects in a database and informs what queries are possible.

## Standard Knowledge Schemas

When building software with SKL, schemas are used for three main purposes:

1. To describe and document data structures
2. To validate that data conforms to specific data structures
3. As configuration detailing what capabilities are possible and how those capabilities are performed

There are three types of SKL Schemas:

* **Nouns**: describe data structures representing concepts commonly used by software tools
* **Verbs**: represent capabilities exposed by software tools (and their parameters and return values)
* **Mappings**: sets of configurations which dictate how a program can translate between Nouns and Verbs and the unique API of any software tool.

The Nouns, Verbs and Mappings of SKL make it so that:

* Data can be understood by what it represents rather than relying on where it comes from or where it is stored. This is done using Nouns.
* A developer, end user, or application can easily discover what Verbs (i.e. software capabilities) can be used over any given piece of data (a Noun) and vice versa.
* A developer, end user, or application can perform operations on/with any given Noun(s) regardless of where the data comes from or how it’s stored.
* Complex workflows, applications, and automations which integrate data from multiple tools (i.e., Integrations) can be easily built and customized with Nouns and Verbs such that you don’t have to interact with tool specific APIs nor create and manage specialized code for each.

### RDF
SKL Schemas are modeled using the Resouces Description Framework ([RDF](https://en.wikipedia.org/wiki/Resource_Description_Framework)), and are most commonly viewed and edited in the [JSON-LD](https://json-ld.org/) format. 