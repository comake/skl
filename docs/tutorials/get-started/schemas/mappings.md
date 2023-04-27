THIS IS OLD AND NOT USED

# Mappings

A Mapping is a set of configuration which dictates how a program can translate between Nouns and Verbs and the unique API of any software tool.

Mappings are the glue which holds the components of SKL together and that allow them to interoperate. By having Mappings defined separately from [Nouns](./nouns.md), [Verbs](./verbs.md), and [Integrations](../../other/terminology.md#integration), users or developers can more easily compose components of the system together.

The schema for a Mapping has a name, a type, and sets of declarative rules that define how Nouns or Verbs are to be transformed. Mappings use the [RDF Mapping Language (RML)](https://rml.io/) to express how data is translated.

SKL includes multiple types of Mappings for translating between different types of components.


## Verb Integration Mapping

A Verb Integration Mapping translates the execution of a Verb into an operation to perform using the API of an Integration. An [SKL Engine]() goes through 4 steps when using a Verb Integration Mapping to execute a Verb:

1. Determine what operation should be performed with the integration. This is done using the `operationMapping` field.
2. Transform the standard parameters of the Verb into the unique parameters of the Integration operation. This is done using the `parameterMapping` and `parameterMappingFrame` fields.
3. Execute the operation!
4. Transform the return value of the operation into the standard return value of the Verb. This is done using the `returnValueMapping` and `returnValueFrame` fields.

A Verb Integration Mapping 

| name | Type | Required | Description | Cardinality |
| ---- | ---- | ---- | ----------- | ---- |
| verb | [Verb](../../../schemas/core/verb) | true | The Verb that the Mapping is used to execute in a specific Integration. | 1..1 |
| integration | [Integration](../../../schemas/core/integration) | true | The Integration who's API operation the Mapping is used to translate a Verb execution into. | 1..1 |
| operationMapping | [TriplesMap](http://www.w3.org/ns/r2rml#TriplesMap) | false | An RML TriplesMap specifying how the parameters of the Verb should be used to determine the name and type of the operation to perform in the Integration. | 0..* |
| parameterMapping | [TriplesMap](http://www.w3.org/ns/r2rml#TriplesMap) | false | An RML TriplesMap specifying how the standard parameters of the Verb should be translated into the unique parameters of an Integration operation. | 0..* |
| parameterMappingFrame | [JSON](http://www.w3.org/1999/02/22-rdf-syntax-ns#JSON) | false | A JSON-LD Frame used to transform the JSON-LD returned by the parameterMapping into the format required by the Integration's operation. | 0..* |
| returnValueMapping | [TriplesMap](http://www.w3.org/ns/r2rml#TriplesMap) | false | An RML TriplesMap specifying how the unique return value of the Integration should be translated into the standard return value of the Verb. | 0..* |
| returnValueFrame | [JSON](http://www.w3.org/1999/02/22-rdf-syntax-ns#JSON) | false | A JSON-LD Frame used to transform the JSON-LD returned by the returnValueMapping into a prefered format. This field overrides the returnValueFrame of the Verb. If not supplied, the Verb's returnValueFrame will be used instead. | 0..1 |

## Noun Verb Mapping



**Noun-Interface Mapping - 🚧 Under development**

A Noun-Interface Mapping is a type of Data-Noun Mapping translates a Noun into the inputs of an interface component.

**Example**

You can see an example of the Verb-Integration Mapping which translates between the [`getFilesInFolder`](https://github.com/comake/skl-dictionary/blob/main/verbs/getFilesInFolder/schema.json) Verb and the [Dropbox](https://github.com/comake/skl-dictionary/blob/main/integrations/dropbox/schema.json) Integration [here in the SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/mappings/getFilesInFolderToDropbox/schema.json).

<!-- **Data-Noun Mapping**

A Data-Noun Mapping either translates an entity conforming to the schema of a Noun into a unique data structure (eg. to be used as the parameters of an Integration’s API), or translates a unique data structure (eg. returned from an Integration’s API) into the schema of a Noun. Data-Noun Mappings are often used by reference by Verb-Integration Mappings to avoid duplication of Mappings. -->