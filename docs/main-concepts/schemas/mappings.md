# Mappings

A Mapping is a set of configuration which dictates how a program can translate between Nouns and Verbs and the unique API of any software tool.

Mappings are the glue which holds the components of SKL together and that allow them to interoperate. By having Mappings defined separately from [Nouns](./nouns.md), [Verbs](./verbs.md), and [Integrations](../../other/terminology.md#integration), users or developers can more easily compose components of the system together.

The schema for a Mapping has a name, a type, and sets of declarative rules that define how Nouns or Verbs are to be transformed. Mappings use the [RDF Mapping Language (RML)](https://rml.io/) to express how data is translated.

SKL includes multiple types of Mappings for translating between different types of components.

**Data-Noun Mapping**

A Data-Noun Mapping either translates an entity conforming to the schema of a Noun into a unique data structure (eg. to be used as the parameters of an Integration’s API), or translates a unique data structure (eg. returned from an Integration’s API) into the schema of a Noun. Data-Noun Mappings are often used by reference by Verb-Integration Mappings to avoid duplication of Mappings.

**Verb-Integration Mapping**

A Verb-Integration Mapping translates the parameters of a Verb to the unique parameters and API endpoint of an Integration to execute and perform the intent of the Verb using the Integration. Verb-Integration Mappings also include a conversion of the response of the API endpoint to the standard return value of the Verb. They may reference one or more Data-Noun Mappings.

**Noun-Interface Mapping - 🚧 Under development**

A Noun-Interface Mapping is a type of Data-Noun Mapping translates a Noun into the inputs of an interface component.

**Example**

You can see an example of the Verb-Integration Mapping which translates between the [`getFilesInFolder`](https://github.com/comake/skl-dictionary/blob/main/verbs/getFilesInFolder/schema.json) Verb and the [Dropbox](https://github.com/comake/skl-dictionary/blob/main/integrations/dropbox/schema.json) Integration [here in the SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/mappings/getFilesInFolderToDropbox/schema.json).
