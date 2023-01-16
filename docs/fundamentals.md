# Fundamentals

Standard Knowledge Language (SKL) allows developers to interact with Integrations - any software tool that has data and capabilities accessible via an API - without having to create and manage specialized code for each.

To use SKL a developer needs two things:
- Standard Knowledge Schemas
- A Standard Knowledge Query Language Engine - to read schemas and execute Verbs and Mappings. You can use the prebuilt Javascript Engine or [build your own](./engine.md).

This page focuses on Schemas. You can read more about SKQL Engines on the [Build an SKQL Engine](./engine.md) page.

## Schemas

SKL is a schema validated and configuration driven framework. Standard Knowledge Schemas serve both as schema to ensure data conforms to specific structures, and configuration detailing what capabilities are possible and how those capabilities are performed. Schemas are made up of Nouns, Verbs, and Mappings:
- Data is translated through Nouns.
- Capabilities are represented by Verbs.
- Mappings are used to relate Nouns and Verbs and translate between raw data and capabilities and Nouns and Verbs

Schemas and instances of the schema ([entities](#entities)) follow the principles of [Linked Data](https://www.w3.org/standards/semanticweb/data). As Linked Data, schemas and entities contain links which reference and connect them to each other to form a semantic knowledge graph. They are modelled in the Resource Description Framework (RDF) and are most commonly worked with using the JSON-LD or Turtle serializations.

An example of the schema for Nouns:

{% tabs %} {% tab title="JSON-LD" %}
  ```json
  // JSON-LD @context not included for brevity
  {
    "@id": "https://skl.standard.storage/nouns/Noun",
    "@type": ["owl:Class", "shacl:NodeShape"],
    "rdfs:label": "Noun",
    "shacl:closed": false,
    "shacl:property": {
      "shacl:maxCount": 1,
      "shacl:path": "https://skl.standard.storage/properties/id"
    }
  }
  ```

{% endtab %}
{% tab title="Turtle" %}
  ```turtle
  @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
  @prefix sh: <http://www.w3.org/ns/shacl#> .
  @prefix owl: <http://www.w3.org/2002/07/owl#> .
  @prefix sklnoun: <https://skl.standard.storage/nouns/> .
  @prefix sklproperties: <https://skl.standard.storage/properties/> .

  sklnoun:Noun a owl:Class, sh:NodeShape ;
      rdfs:label "Noun" ;
      sh:closed false ;
      sh:property [ sh:maxCount 1 ;
              sh:path sklproperties:id ] .
  ```
{% endtab %} {% endtabs %}

## Nouns

A Noun is a schema for a data structure representing a concept used by software tools. When you use SKL, everything is translated through a Noun: emails, notes, webpages, files, podcast episodes, news articles, receipts, prescriptions, medications, locations, etc.

Many software tools use data structures with the same name but with slight differences. Nouns are an abstraction of these different data structures that enables a higher degree of interoperability between them. Their schemas include:
1. Canonical fields and capabilities which entities of a Noun are required to support.
2. Fields or capabilities that not required but are enumberated because they may be supported by a majority of Integrations.
3. Any other abritrary fields and capabilities an Integration may support but are not explicitly enumberated in the schema.

For example, [Google Drive](https://www.google.com/drive/), [Dropbox](https://www.dropbox.com/), and [Box](https://www.box.com/) each established a data structure for representing files in their respective system. The developers of these tools each determined the fields used to represent these files and the capabilities they wanted their system to support. For example, access permissions over files in each service are controlled via roles (e.g., viewer, uploader, editor, etc.) that give users slightly different controls over the underlying files and data. Normally, a developer would have to write different code to work with each. Instead, using SKL, the unique data structures of each tool can be translated (see [Mappings](#mappings) for info about the translation process) into instances of the File Noun. The resulting File Entities from each system include common fields like name, mime type, and size, and capabilities such as the ability to rename or move the files in their respective systems. They may also include fields and capabilities only some systems support such as a field containing a unique hash of the contents of the file, which Box and Dropbox support but Google Drive doesn't. Dropbox also has support for adding any arbitrary key/value data to files, this information will be included in any File Entities from Dropbox.

In this way, Nouns are abstractions of the commonalities between each Integration’s representation of a concept but are also extendable and customizable, allowing the unique fields each tool has to still exist on entities and unique capabilities to still be used.

##### Entities {#entities}

An entity is an instance of a Noun which conforms to the Noun's schema.

##### Properties

A Property of a Noun is a characteristic that has a label and a value, such as the `name` property of a File, or the `status` property of a Task. Each property has a schema that defines its name, description, type, default value, range, cardinality, length or pattern.

##### SHACL

The schema that entities of a Noun must conform to is defined using the [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/).

An example of the schema for the File Noun:

{% tabs %} {% tab title="JSON-LD" %}
  ```json
  // JSON-LD @context not included for brevity
  {
    "@id": "https://skl.standard.storage/nouns/File",
    "@type": ["owl:Class", "shacl:NodeShape"],
    "rdfs:label": "File",
    "rdfs:subClassOf": "https://skl.standard.storage/nouns/Noun",
    "skos:definition": "An electronic document.",
    "shacl:closed": false,
    "shacl:property": [
      {
        "shacl:maxCount": 1,
        "shacl:minCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/name"
      },
      {
        "shacl:maxCount": 1,
        "shacl:minCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/sourceId"
      },
      {
        "shacl:class": "https://skl.standard.storage/nouns/Integration",
        "shacl:maxCount": 1,
        "shacl:minCount": 1,
        "shacl:nodeKind": { "@id": "shacl:IRI" },
        "shacl:path": "https://skl.standard.storage/properties/integration"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/size"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/url"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/md5Checksum"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/mimeType"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/isWeblink"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/deleted"
      }
    ]
  }
  ```
{% endtab %}
{% tab title="Turtle" %}
  ```turtle
  @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
  @prefix skos: <http://www.w3.org/2004/02/skos/core#> .
  @prefix sh: <http://www.w3.org/ns/shacl#> .
  @prefix owl: <http://www.w3.org/2002/07/owl#> .
  @prefix sklnoun: <https://skl.standard.storage/nouns/> .
  @prefix sklproperties: <https://skl.standard.storage/properties/> .

  sklnoun:File a owl:Class, sh:NodeShape ;
      rdfs:label "File" ;
      rdfs:subClassOf sklnoun:Noun ;
      skos:definition "An electronic document." ;
      sh:closed false ;
      sh:property [ sh:maxCount 1 ;
              sh:minCount 1 ;
              sh:path sklproperties:name ],
          [ sh:maxCount 1 ;
              sh:minCount 1 ;
              sh:path sklproperties:sourceId ],
          [ sh:class sklnoun:Integration ;
              sh:maxCount 1 ;
              sh:minCount 1 ;
              sh:nodeKind sh:IRI ;
              sh:path sklproperties:integration ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:size ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:url ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:md5Checksum ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:mimeType ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:isWeblink ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:deleted ] .
  ```
{% endtab %} {% endtabs %}

{% hint style="success" %} Setting "shacl:closed" to false means that data conforming to the schema can include any arbitrary properties not explicitly enumerated as a property in the schema. {% endhint %}

{% hint style="info" %} We acknowledge that it's impossible to create a single ontology that can appease every use case. In addition, it's a huge undertaking to attempt to model an abstraction for every data structure a software tool may need to work with. In light of these facts, SKL Schemas are highly modular, customizable, and are evaluated at runtime to any enable capabilities an application or end user may need. In addition, in the coming weeks, SKL will publish the Dictionary, which is an open source Library of Nouns, Verbs, and Mappings which developers can use and contribute to. {% endhint %}

## Verbs

A Verb is a representation of a capability exposed by software tools.

Software tools often expose similar capabilities that vary slightly in their parameters, outputs, and execution methods, even though they may have the same meaning or eventual effect. Verbs create a simplified way through which developers can use the general capabilities of software tools without having to know the specific requirements or formats of each tool's API.

For example, let's say a developer is building a productivity application which displays a user's files from [Dropbox](https://www.dropbox.com/), saved articles from [Medium](https://medium.com/), and tasks from [Asana](https://asana.com/). The developer could build into their application a single interface to "share" any of these files, articles, or tasks with a co-worker. When a user shares something through this interface, the developer's code can use the `share` Verb with the entity to be shared and a Person entity (who the entity is being shared with) as parameters. The relevant Mappings (defined below) can then be used to translate the parameters into a properly formatted request to send to the Dropbox, Medium, or Asana API depending on the type and source Integration of the entity being shared.

In this way, Verbs are intelligently executed according to the context in which they are used. This is similar to how the meaning of a word can be understood differently based on how it's used in a sentence.

Verbs often return results. For example, the `getFilesInFolder` Verb returns a collection of File entities for any given Folder. In the same way that Mappings are used to translate the standard parameters of a Verb into the correct operation to perform, the outputs of the operation are translated using Mappings into a standard output.

In addition to being explicitly called by a developer's code, Verbs may be configured to be run at a specific time, upon a specific schedule, or in response to specific events. For example, a Verb can specify that it should be run in response to an event from a webhook registered with the API of an Integration.

{% hint style="info" %}
**Coming soon**: Composite Verbs - Combine a series of Verbs to form larger processes!
{% endhint %}

The schema for a Verb:

{% tabs %} {% tab title="JSON-LD" %}
  ```json
  // JSON-LD @context not included for brevity
  {
    "@id": "https://skl.standard.storage/nouns/Verb",
    "@type": ["owl:Class", "shacl:NodeShape"],
    "rdfs:label": "Verb",
    "shacl:closed": false,
    "shacl:property": [
      {
        "shacl:class": "shacl:NodeShape",
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/returnType"
      },
      {
        "shacl:class": "shacl:NodeShape",
        "shacl:maxCount": 1,
        "shacl:nodeKind": { "@id": "shacl:BlankNode" },
        "shacl:path": "https://skl.standard.storage/properties/parameters"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/name"
      },
      {
        "shacl:maxCount": 1,
        "shacl:path": "https://skl.standard.storage/properties/id"
      }
    ]
  }
  ```
{% endtab %}
{% tab title="Turtle" %}
  ```turtle
  @prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
  @prefix sh: <http://www.w3.org/ns/shacl#> .
  @prefix owl: <http://www.w3.org/2002/07/owl#> .
  @prefix sklnoun: <https://skl.standard.storage/nouns/> .
  @prefix sklproperties: <https://skl.standard.storage/properties/> .

  sklnoun:Verb a owl:Class, sh:NodeShape ;
      rdfs:label "Verb" ;
      sh:closed false ;
      sh:property [ sh:class sh:NodeShape ;
              sh:maxCount 1
              sh:path sklproperties:returnType ],
          [ sh:class sh:NodeShape ;
              sh:maxCount 1
              sh:nodeKind sh:BlankNode ;
              sh:path sklproperties:parameters ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:name ],
          [ sh:maxCount 1 ;
              sh:path sklproperties:id ] .
  ```
{% endtab %} {% endtabs %}

An example of the schema for the `getFilesInFolder` Verb:

{% tabs %} {% tab title="JSON-LD" %}
  ```json
  // JSON-LD @context not included for brevity
  {
    "@id": "https://skl.standard.storage/verbs/getFilesInFolder",
    "@type": "https://skl.standard.storage/nouns/Verb",
    "sklproperties:name": "getFilesInFolder",
    "sklproperties:parameters": {
      "@type": "shacl:NodeShape",
      "shacl:closed": true,
      "shacl:property": [
        {
          "shacl:datatype": "xsd:integer",
          "shacl:maxCount": 1,
          "shacl:name": "limit",
          "shacl:path": "https://skl.standard.storage/properties/limit"
        },
        {
          "shacl:datatype": "xsd:string",
          "shacl:maxCount": 1,
          "shacl:name": "token",
          "shacl:path": "https://skl.standard.storage/properties/token"
        },
        {
          "shacl:class": "https://skl.standard.storage/nouns/Folder",
          "shacl:maxCount": 1,
          "shacl:minCount": 1,
          "shacl:name": "folder",
          "shacl:path": "https://skl.standard.storage/properties/folder"
        },
        {
          "shacl:class": "https://skl.standard.storage/nouns/Account",
          "shacl:maxCount": 1,
          "shacl:minCount": 1,
          "shacl:name": "account",
          "shacl:path": "https://skl.standard.storage/properties/account"
        }
      ]
    },
    "sklproperties:returnValue": "https://skl.standard.storage/nouns/TokenPaginatedCollection"
  }
  ```
{% endtab %}
{% tab title="Turtle" %}
  ```turtle
  @prefix sklproperties: <https://skl.standard.storage/properties/> .
  @prefix sklnouns: <https://skl.standard.storage/nouns/> .
  @prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
  @prefix sh: <http://www.w3.org/ns/shacl#> .

  <https://skl.standard.storage/verbs/getFilesInFolder> a sklnouns:Verb ;
      sklproperties:name "getFilesInFolder" ;
      sklproperties:parameters [ a sh:NodeShape ;
          sh:closed true ;
          sh:property [ sh:maxCount 1 ;
                  sh:name "limit" ;
                  sh:datatype xsd:integer ;
                  sh:path sklproperties:limit ],
              [ sh:maxCount 1 ;
                  sh:name "token" ;
                  sh:datatype xsd:string ;
                  sh:path sklproperties:token ],
              [ sh:class sklnouns:Folder ;
                  sh:maxCount 1 ;
                  sh:minCount 1 ;
                  sh:name "folder" ;
                  sh:path sklproperties:folder ],
              [ sh:class sklnouns:Account ;
                  sh:maxCount 1 ;
                  sh:minCount 1 ;
                  sh:name "account" ;
                  sh:path sklproperties:account ] ] ;
      sklproperties:returnValue sklnouns:TokenPaginatedCollection .
  ```
{% endtab %} {% endtabs %}

## Mappings {#mappings}

A Mapping is a set of configuration which dictates how a program can translate between Nouns and Verbs and the unique API of any software tool.

Mappings are the glue which holds the components of SKL together and that allow them to interoperate. By having Mappings defined separately from Nouns, Verbs, and Integrations, users or developers can more easily compose components of the system together.

The schema for a Mapping has a name, a type, and sets of declarative rules that define how Nouns or Verbs are to be transformed. Mappings use the [RDF Mapping Language (RML)](https://rml.io/) to express how data is translated.

SKL includes multiple types of Mappings for translating between different types of artifacts.

##### Data-Noun Mapping
A Data-Noun Mapping either translates an entity conforming to the schema of a Noun into a unique data structure (eg. to be used as the parameters of an Integration’s API), or translates a unique data structure (eg. returned from an Integration’s API) into the schema of a standard Noun. Data-Noun Mappings are often used by reference by by VerbIntegrationMappings to avoid duplication of Mappings.

##### Verb-Integration Mapping
A Verb-Integration Mapping translates the parameters of a Verb to the unique parameters and API endpoint of an Integration to execute and perform the intent of the Verb using the Integration. Verb-Integration Mappings also include a conversion of the response of the API endpoint to the standard return value of the Verb. They may reference one or more Data-Noun Mappings.

##### Ontology Mapping - Coming Soon
An Ontology Mapping is a type of Data-Noun Mapping that describes how to map the schema and structure of a different Linked Data ontology to an SKL Noun.

##### Noun-Interface Mapping - Coming Soon
A Noun-Interface Mapping is a type of Data-Noun Mapping translates a Noun into the inputs of an interface component.

##### Verb-QueryLanguage Mapping - Coming Soon
A Verb-QueryLanguage Mapping translates the inputs of a standard Verb to a query in a database language and the outputs of the query back to the standard outputs of the Verb. They may reference one or more Data-Noun Mappings.

An example of the Verb-Integration Mapping which translates between the `getFilesInFolder` Verb and the Dropbox Integration:

{% tabs %} {% tab title="JSON-LD" %}
  ```json
  // JSON-LD @context not included for brevity
  {
    "@id": "https://skl.standard.storage/data/4",
    "@type": "https://skl.standard.storage/nouns/VerbIntegrationMapping",
    "integration": "https://skl.standard.storage/integrations/Dropbox",
    "name": "getFilesInFolderToDropbox",
    "operationMappings": "https://skl.standard.storage/data/4/operationMappings/1",
    "parameterMappings": "https://skl.standard.storage/data/4/parameterMappings/1",
    "returnValueMappings": [
      "https://skl.standard.storage/data/4/returnValueMappings/1",
      "https://skl.standard.storage/data/4/returnValueMappings/2"
    ],
    "verb": "https://skl.standard.storage/verbs/getFilesInFolder"
  }
  ```
{% endtab %}
{% tab title="Turtle" %}
  ```turtle
  @prefix sklproperties: <https://skl.standard.storage/properties/> .
  @prefix sklnouns: <https://skl.standard.storage/nouns/> .

  <https://skl.standard.storage/data/4> a sklnouns:VerbIntegrationMapping ;
      sklproperties:parameterMappings <https://skl.standard.storage/data/4/parameterMappings/1> ;
      sklproperties:integration <https://skl.standard.storage/integrations/Dropbox> ;
      sklproperties:name "getFilesInFolderToDropbox" ;
      sklproperties:operationMappings <https://skl.standard.storage/data/4/operationMappings/1> ;
      sklproperties:returnValueMappings <https://skl.standard.storage/data/4/returnValueMappings/1>,
          <https://skl.standard.storage/data/4/returnValueMappings/2> ;
      sklproperties:verb <https://skl.standard.storage/verbs/getFilesInFolder> .
  ```
{% endtab %} {% endtabs %}
