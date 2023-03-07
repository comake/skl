# Nouns

A Noun is a schema for a data structure representing a concept used by software tools. When you use SKL, everything is translated through a Noun: emails, notes, webpages, files, podcast episodes, news articles, receipts, prescriptions, medications, locations, etc.

Many software tools use data structures with the same name but with slight differences. Nouns are an abstraction of these different data structures that enables a higher degree of interoperability between them.

## Example

The developers of [Google Drive](https://www.google.com/drive/), [Dropbox](https://www.dropbox.com/), and [Box](https://www.box.com/) each defined a different data structure to represent files in their respective software and APIs. Normally, when integrating files from these tools into an application, a developer would have to write different code to work with each. Instead, using SKL, the unique data structures of each tool can be translated (via [Mappings](https://github.com/comake/skl/blob/main/fundamentals/README.md#mappings)) into instances of the File Noun. The resulting File entities from each system include common fields like name, mime type, and size. They may also include fields that only some systems support such as a unique hash of the contents of the file, which Box and Dropbox support but Google Drive doesn't. Dropbox also has support for adding any arbitrary key/value data to files, this information will be included in any File entities from Dropbox.

In this way, Nouns are abstractions of the commonalities between each Integration’s representation of a concept but are also extendable and customizable, allowing the unique fields each tool has to still exist on entities.

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

This NodeShape states that every Noun must have a label, and may optionally have a description and a JSON-LD context. You can see the documentation for this Noun Schema in the [SKL Dictionary](https://github.com/comake/skl-dictionary/tree/main/schemas/core/noun).

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
      "shacl:path": "skl:size",
      "shacl:name": "size"
    },
    {
      "shacl:maxCount": 1,
      "shacl:path": "skl:mimeType",
      "shacl:name": "mimeType"
    },
    // ... 
  ]
}
```

You can see the full example of the File noun in the [SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/schemas/nouns/file/schema.json).

{% hint style="success" %}
Most Noun schemas have the `shacl:closed` property set to `false`. This means that data conforming to the schema can include any arbitrary properties not explicitly enumerated as a property in the schema.
{% endhint %}

{% hint style="info" %}
We acknowledge that it's impossible to create a single ontology that can appease every use case. In addition, it's a huge undertaking to attempt to model an abstraction for every data structure a software tool may need to work with. In light of these facts, SKL Schemas are highly modular, customizable, and are evaluated at runtime to enable any capabilities an application or end user may need. In addition, we are continuously adding to the \[SKL Dictionary]\(https://github.com/comake/skl-dictionary), an open source Library of Nouns, Verbs, and Mappings which developers can use and contribute to.
{% endhint %}
