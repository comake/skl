# Mappings

## What is a Mapping?

A Mapping is a set of configuration which dictates how a program can translate between Nouns and Verbs and the unique API of any Integration.

Mappings are the glue which holds the components of SKL together and that allow them to interoperate. By having Mappings defined separately from [Nouns](./nouns.md), [Verbs](./verbs.md), and [Integrations](./integrations.md), users or developers can more easily compose components of the system together.

The schema for a Mapping has a name, a type, and sets of declarative rules that define how Nouns or Verbs are to be transformed. Mappings use the [RDF Mapping Language (RML)](https://rml.io/) to express how data is translated.

SKL includes multiple types of Mappings for translating between different types of components. They are:

### Verb-Integration Mapping

A Verb-Integration Mapping translates the execution of a Verb into an operation to perform using the API of an Integration. 

### Verb-Noun Mapping

A Verb-Noun Mapping translates the execution of a Verb into another Verb based on a provided `noun` parameter.

### Noun-Interface Mapping - 🚧 Under development

A Noun-Interface Mapping translates a Noun into the inputs of an interface component.

### Trigger-Verb Mapping - 🚧 Under development

A Trigger-Verb Mapping translates the receipt of a Trigger (eg. an event or message) into the execution of a specific Verb.