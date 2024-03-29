THIS IS OLD AND NOT USED

# Nouns

## What is a Noun?

A Noun is a schema for a data structure representing a concept used by Integrations. When you use SKL, everything is translated through a Noun: emails, notes, webpages, files, podcast episodes, news articles, receipts, prescriptions, medications, locations, etc.

Many software tools use data structures with the same name but with slight differences. Nouns are an abstraction of these different data structures that enables a higher degree of interoperability between them.

## Example

The developers of [Google Drive](https://www.google.com/drive/), [Dropbox](https://www.dropbox.com/), and [Box](https://www.box.com/) each defined a different data structure to represent files in their respective software and APIs. Normally, when integrating files from these tools into an application, a developer would have to write different code to work with each. 

Instead, using SKL, the unique data structures of each tool can be translated (via [Mappings](./mappings.md)) into instances of the File Noun. The resulting File entities from each system include common properties like name, mime type, and size. They may also include properties that only some systems support such as a unique hash of the contents of the file, which Box and Dropbox support but Google Drive doesn't. Dropbox also has support for adding any arbitrary key/value data to files, this information will be included in any File entities from Dropbox.

In this way, Nouns are abstractions of the commonalities between each Integration’s representation of a concept but are also extendable and customizable, allowing the unique properties each tool has to still exist on entities.

{% hint style="info" %}
We acknowledge that it's impossible to create a single ontology that can appease every use case. In addition, it's a huge undertaking to attempt to model an abstraction for every data structure a software tool may need to work with. In light of these facts, SKL is highly modular, customizable, and it's components are evaluated at runtime to enable any capabilities an application or end user may need. In addition, we are continuously adding to the \[SKL Dictionary]\(https://github.com/comake/skl-dictionary), an open source Library of Nouns, Verbs, and Mappings which developers can use and contribute to.
{% endhint %}

## Entity

An entity is an instance of a Noun which conforms to the Noun's schema.

## Properties

A Property of a Noun is a characteristic that has a label and a value, such as the `title` property of a File, or the `status` property of a Task. Each property has a schema that can defines its name, description, type, default value, range, cardinality, length or pattern. Properties of nouns can be required (having a minimim cardinality of one).

## SHACL

SKL Nouns use the [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/) to define their properties.

Every Noun conforms to the Noun SHACL [NodeShape](https://www.w3.org/TR/shacl/#node-shapes):
```json
{
  "@context": { /* context omitted for brevity*/ },
  "@id": "https://standardknowledge.com/ontologies/core/Noun",
  "@type": ["owl:Class", "shacl:NodeShape"],
  "label": "Noun",
  "description": "A data structure representing a concept commonly used by software tools.",
  "shacl:closed": false,
  "shacl:property": [
    {
      "shacl:maxCount": 1,
      "shacl:minCount": 1,
      "shacl:name": "label",
      "shacl:path": "rdfs:label",
      "shacl:datatype": "xsd:string",
      "shacl:description": "A human readable identifier for this Noun."
    },
    {
      "shacl:maxCount": 1,
      "shacl:name": "description",
      "shacl:path": "dcterms:description",
      "shacl:datatype": "xsd:string",
      "shacl:description": "A description of what this Noun represents."
    },
    {
      "shacl:maxCount": 1,
      "shacl:name": "context",
      "shacl:path": "skl:context",
      "shacl:datatype": "rdf:JSON",
      "shacl:description": "A JSON-LD Context Definition used to compact entities of this noun into a more human readable format"
    }
  ]
}
```

Using code, this NodeShape can be transformed into a table that describes the Noun Schema:

| name | Type | Required | Description | Cardinality |
| ---- | ---- | ---- | ----------- | ---- |
| label | [string](http://www.w3.org/2001/XMLSchema#string) | true | A human readable identifier for this Noun. | 1..1 |
| description | [string](http://www.w3.org/2001/XMLSchema#string) | false | A description of what this Noun represents. | 0..1 |
| context | [JSON](http://www.w3.org/1999/02/22-rdf-syntax-ns#JSON) | false | A JSON-LD Context Definition used to compact entities of this noun into a more human readable format | 0..1 |

As you can see, this NodeShape states that every Noun must have a label, and may optionally have a description and a JSON-LD context. You can see the full documentation for the Noun Schema in the [SKL Dictionary](https://github.com/comake/skl-dictionary/tree/main/schemas/core/noun).

In addition to conforming to the Noun Schema, every Noun is also a SHACL NodeShape and an OWL Class. For example, here is a shortened version of a File Noun:

```json
{
  "@context": { /* context omitted for brevity*/ },
  "@id": "https://example.com/File",
  "@type": ["owl:Class", "shacl:NodeShape", "skl:Noun"],
  "label": "File",
  "description": "An electronic document.",
  "shacl:closed": false,
  "shacl:property": [
    {
      "shacl:maxCount": 1,
      "shacl:minCount": 1,
      "shacl:path": "rdfs:label",
      "shacl:name": "label"
    },
    {
      "shacl:maxCount": 1,
      "shacl:path": "example:size",
      "shacl:name": "size"
    },
    {
      "shacl:maxCount": 1,
      "shacl:path": "example:mimeType",
      "shacl:name": "mimeType"
    }
  ],
  "skl:context" {
    // ...
  }
}
```

You can see the full example of the File noun in the [SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/schemas/nouns/file/schema.json).

{% hint style="success" %}
Most Noun schemas have the `shacl:closed` property set to `false`. This means that data conforming to the schema can include any arbitrary properties not explicitly enumerated as a property in the schema.
{% endhint %}

## Context

Every Noun can have a JSON-LD context so that entities of the Noun can be [compacted](https://www.w3.org/TR/json-ld11/#compacted-document-form) into a more human-readable form. This is useful when developers of APIs or other query interfaces use SKL and store entities as JSON-LD but want to expose entities to users or other software in a format they are more familiar with.

An shortened example of the [File](https://github.com/comake/skl-dictionary/blob/main/schemas/nouns/file/schema.json) Noun's context and expanded and compacted examples of a File entity:
```json
// The File Noun's skl:context property:
{
  "label": {
    "@id": "http://www.w3.org/2000/01/rdf-schema#label",
    "@type": "http://www.w3.org/2001/XMLSchema#string"
  },
  "size": {
    "@id": "https://example.com/size",
    "@type": "http://www.w3.org/2001/XMLSchema#integer"
  },
  "mimeType": {
    "@id": "https://example.com/mimeType",
    "@type": "http://www.w3.org/2001/XMLSchema#string"
  }
}

// Expanded entity form:
{
  "@id": "https://example.com/data/1",
  "@type": "https://example.com/File",
  "http://www.w3.org/2000/01/rdf-schema#label": {
    "@type": "http://www.w3.org/2001/XMLSchema#string",
    "@value": "presentation_final.pptx"
  },
  "https://example.com/size": {
    "@type": "http://www.w3.org/2001/XMLSchema#integer",
    "@value": "156"
  },
  "https://example.com/mimeType": {
    "@type": "http://www.w3.org/2001/XMLSchema#string"
    "@value": "application/vnd.openxmlformats-officedocument.presentationml.presentation",
  },
}

// Compacted entity form:
{
  "@context": { /* context omitted for brevity (displayed above) */ },
  "@id": "https://example.com/data/1",
  "@type": "https://example.com/File",
  "label": "presentation_final.pptx",
  "size": "156",
  "mimeType": "application/vnd.openxmlformats-officedocument.presentationml.presentation"
}

```

Any tool which implements the [JSON-LD](https://json-ld.org/) specification, such as [jsonld.js](https://github.com/digitalbazaar/jsonld.js), can be used to perform compaction or expansion.
