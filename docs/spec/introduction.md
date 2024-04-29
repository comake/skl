# Specification

The open-source protocol for integrating data and capabilities from software tools.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## Introduction

Standard Knowledge Language (SKL) specifies how developers can define schemas representing the data and capabilities of software tools and how those schemas can be used to build applications, workflows, and automations. It is platform agnostic, so you can use it to build applications in any programming language. 

This document describes the Standard Knowledge Language and its concepts through definitions and examples.

## Terminology

#### Noun
A Noun is an abstraction of a type of data structure used by the APIs of software tools (eg. File, Person, Task, Patient, etc.). Many software tools use data structures with the same name but with slight differences. Nouns create a shared middle ground which developers can use instead of writing code to interact with each software tool’s unique data structures. Nouns are extendable and customizable, allowing developers to add fields for their unique needs. A Noun can have fields that are not supported by every tool that has mappings to it.

#### Verb
A Verb is a representation of a capability exposed by a software tool (eg. Share, Send, Download, Like, etc.). Like their unique data structures, software tools expose similar capabilities that vary slightly in their inputs, outputs, and execution methods, even though they may have the same meaning or eventual effect. Verbs create a simplified way through which developers can use the capabilities of software tools without having to know the distinct requirements of each.

#### Mapping
A Mapping is a configuration which specifies how how a Noun can be translated to or from a unique data format specific to an Integration, or how a Verb can be translated to or from a unique capability of a software tool.

#### Entity
An Entity is an instance of data conforming to a Noun schema, often corresponding to a thing or capability in the real world.

#### Schema
A Schema is a description of an SKL component. Schemas are written declaratively, making it so that they can be used in any programming environment. They are modelled using the Resouce Description Framework ([RDF](https://en.wikipedia.org/wiki/Resource_Description_Framework)).

#### Integration
A software tool which has data and capabilities accessible via one or more APIs that SKL can communicate with to query for, mutate, receive messages about, or otherwise access or perform actions over data.

#### Account
An Account is a representation of a person, company, or other entity's registration and use of the serivces or interfaces of an Integration. 

#### SKL Engine
A library of code that can read SKL Schemas and execute Verbs and Mappings. An Engine's main requirements are that developers using it can set a source of schemas and execute Verbs.

#### URI
A Uniform Resource Identifier (URI) provides the means for identifying resources [RFC3986](https://datatracker.ietf.org/doc/html/rfc3986).

## Namespaces

In this document, examples assume the following namespace prefix bindings unless otherwise stated:

| Prefix | IRI |
| ---- | ---- |
| skl: | https://standardknowledge.com/ontologies/core/ |
| rdf: | http://www.w3.org/1999/02/22-rdf-syntax-ns# |
| rdfs: | http://www.w3.org/2000/01/rdf-schema# |
| shacl: | http://www.w3.org/ns/shacl# |
| fnml: | http://semweb.mmlab.be/ns/fnml# |
| dcelements: | http://purl.org/dc/elements/1.1/ |
| dcterms: | http://purl.org/dc/terms/ |
| rr: | http://www.w3.org/ns/r2rml# |
| rml: | http://semweb.mmlab.be/ns/rml# |
| owl: | http://www.w3.org/2002/07/owl# |
| xsd: | http://www.w3.org/2001/XMLSchema# |
| schema: | https://schema.org/ |

## SKL Ontology

The core SKL Ontology namespace is https://standardknowledge.com/ontologies/core/

```
https://standardknowledge.com/ontologies/core/
```

The core SKL Ontology preferred prefix is `skl`.

## Schemas

A project which uses SKL contains or accesses a collection of SKL Schemas which define the data formats and capabilities the project uses. Schemas may describe anything about the project from the types of data it displays, how it displays that data, user accounts, the permissions of users and capabilities they have over data, what infrastructure hosts the project, etc.

SKL Schemas are modeled using the Resouce Description Framework ([RDF](https://en.wikipedia.org/wiki/Resource_Description_Framework)), and are most commonly viewed and edited in the [JSON-LD](https://json-ld.org/) format. As RDF, Schemas are identified by and relate to one another via URI. This forms a graph of the data formats, resources, and/or capabilities a project uses.

The SKL Ontology consists of SKL specific classes, some of which use other ontologies and specificiations such as [RML](https://rml.io/specs/rml/), [SHACL](https://www.w3.org/TR/shacl/), and [OpenAPI](https://spec.openapis.org/oas/v3.1.0).

A project which uses SKL typically begins with stakeholders defining the data types, or **Nouns**, necessary for the system they want to build and the **Integrations** that the data will come from or be submitted to. Next, they define **Verbs** which represent the capabilities exposed by the API of one or more **Integrations**. Finally, they create **Mappings** to translate data between **Nouns** and **Verbs** and the unique API of any **Integration**.

These **Nouns**, **Integrations**, **Verbs**, and **Mappings**, collectively make up a project's SKL Schemas. Schemas are typically stored in a graph database so that they can be referenced and queried by their associations with eachother. Alternatively, implementers may store schemas as JSON-LD files which get loaded into memory whenever some temporary program is run.

## Nouns

A Noun Schema is an RDF Node which defines a data structure representing a concept relevant to a project or application. It can be used by an [Engine](#engines) to validate entities or to generate data. A Noun consists of the following properties:

| name | predicate | description |
| ---- | --------- | ----------- |
| label | `rdfs:label` | **REQUIRED**. A human readable identifier for the Noun. |
| description | `dcelements:description` | A semantic description what the Noun represents. |
| properties | `shacl:property` | Specification of properties that entities of the noun MUST conform to. |
| closed | `shacl:closed` | Whether entities of the Noun can have properties other than the ones explicitly defined in properties. |
| context | `skl:context` | A JSON-LD Context Definition used to compact entities of this noun into a more human readable JSON format. |

SKL Nouns use the [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/) to define their properties. Every Noun is a [`shacl:NodeShape`](https://www.w3.org/TR/shacl/#node-shapes). As a NodeShape, a Noun specifies constaints for its properties in the `shacl:property` field. Each property constraint is a [`shacl:PropertyShape`](https://www.w3.org/TR/shacl/#property-shapes). The fields of each `shacl:PropertyShape` typically follow this format:

| predicate | description |
| ---- | ---- |
| [`shacl:path`](https://www.w3.org/TR/shacl/#property-paths) | **REQUIRED**. A reference to the URI of the property. In most cases, paths are predicate paths, not sequence paths, alternate paths, or any of the other SHACL path types. |
| [`shacl:datatype`](https://www.w3.org/TR/shacl/#DatatypeConstraintComponent) | The data type of a literal valued property. Eg. `xsd:string`, `xsd:boolean`, `xsd:integer`, `xsd:double`, `xsd:dateTime`, or `rdf:JSON` |
| [`shacl:nodeKind`](https://www.w3.org/TR/shacl/#NodeKindConstraintComponent) | If the value of a property is a nested object, this field should be set to `shacl:BlankNode`. If a property is supposed to reference another entity, this field should be set to `shacl:IRI`. |
| [`shacl:class`](https://www.w3.org/TR/shacl/#x4.1.1-sh:class) | In the case that a property is a nested object, this field can be used to specify the `@type` that the object should have. For example, the `schema:Place` Noun has a `schema:address` property with `shacl:class` set to `schema:PostalAddress` to indicate that the nested object is a PostalAddress. |  |
| [`shacl:maxCount`](https://www.w3.org/TR/shacl/#MaxCountConstraintComponent) | The maximum number of values that can be set for this property. If the property is meant to be single valued, set to `1`. If the property is meant to be an array of values don't set this field. |
| [`shacl:minCount`](https://www.w3.org/TR/shacl/#MinCountConstraintComponent) | The minimum number of values that can be set for this property. If the property is required, set this field to `1`. Otherwise, only set this field if there's a specific need to have a certain minimum. |

### Example: Defining a Noun

The following JSON+LD document defines a Noun called "Entity" which can have up to one label, description, date created, and date modified and specifies the data type of each.

```json
{
  "@id": "skl:Entity",
  "@type": ["rdfs:Class", "shacl:NodeShape", "skl:Noun"],
  "label": "Entity",
  "shacl:closed": false,
  "shacl:property": [
    {
      "shacl:maxCount": 1,
      "shacl:path": "rdfs:label",
      "shacl:datatype": "xsd:string"
    },
    {
      "shacl:maxCount": 1,
      "shacl:path": "dcelements:description",
      "shacl:datatype": "xsd:string"
    },
    {
      "shacl:maxCount": 1,
      "shacl:path": "http://purl.org/dc/terms/created",
      "shacl:datatype": "xsd:dateTime"
    },
    {
      "shacl:maxCount": 1,
      "shacl:path": "http://purl.org/dc/terms/modified",
      "shacl:datatype": "xsd:dateTime"
    }
  ]
}
```

## Entity

An **Entity** is an instance of a **Noun** which conforms to the **Noun's** SHACL schema. Every SKL Entity is a [SHACL instance](https://www.w3.org/TR/shacl/#dfn-shacl-instance), which is described by, should conform to, will get validated against as **Noun**.

As seen above, every **Entity** used in SKL typically has the following properties:

| name | predicate | description |
| ---- | --------- | ----------- |
| label | `rdfs:label` | **REQUIRED**. A human readable identifier for the Noun. |
| description | `dcelements:description` | A semantic description what the Noun represents. |
| properties | `shacl:property` | Specification of properties that entities of the noun MUST conform to. |
| createdAt | `dcterms:created` | A timestamp of when the Entity was first generated or saved in a database. |
| updatedAt | `dcterms:modified` | A timestamp of when the Entity was most recently updated. |

## Integrations

An **Integration** is a software tool which has data and capabilities accessible via an API that can be communicated with to query for, mutate, receive messages about, or otherwise access or perform actions over data.

The schema for Integrations is defined by a **Noun**. It includes 

| name | predicate | description |
| ---- | --------- | ----------- |
| label | `rdfs:label` | **REQUIRED**. A human readable identifier for the **Integration**. |
| description | `dcelements:description` | A semantic description what the **Integration** represents. |


### Example: An Integration

```json
{
    "@id": "https://mygraph.standard.storage/integrations/GoogleVertexAI",
    "@type": "https://standardknowledge.com/ontologies/core/Integration",
    "rdfs:label": "GoogleVertexAI",
    "dcelements:description": "Creating safe AGI that benefits all of humanity."
}
```

The schemas for **Integrations** are quite sparse because they are typically used as an identifier which many other schemas relate or refer to:

- `skl:Account` is the class representing people, companies, or other entities' registration and use of the serivces or interfaces of an **Integration**. 
- `skl:SecurityCredentials` is the class of stored credentials to use when performing operations with the API of an **Integration** using a specific **Account**.
- `skl:OpenApiDescription` is the class of OpenAPI descriptions documenting the REST API of an **Integration**
-  `skl:JsonDataSource` is the class of JSON Data Sources related to an **Integration**

<!-- TODO: move invalidTokenErrorMatcher to Account or OpenAPIDescription -->

## Accounts

An Account is a representation of a person, company, or other entity's registration and use of the serivces or interfaces of an Integration. 

Accounts help divide the data we work with into silos to ensure we have the desired authorization, security, attribution, and focus. In SKL, everything done with an **Integration's** APIs happens using a specific Account. 

The schema for Integrations is defined by a **Noun**. It includes:

| name | predicate | description |
| ---- | --------- | ----------- |
| label | `rdfs:label` | **REQUIRED**. A human readable identifier for the **Account**. |
| description | `dcelements:description` | A semantic description what the **Account** represents. |
| integration | `skl:integration` | The **Integration** the *Account** is associated with |
| email | `schema:email` | The email address associated with the **Account**. |
| overrideBasePath | `skl:overrideBasePath` | A URI to override the `servers` setting of an **OpenAPIDescription** when using this **Account**. |

<!-- TODO: add a note about adding things like username -->

## Security Credentials

Coming soon...

## Open API Description

Coming soon...

## JsonDataSource

Coming soon...

## Verbs

Coming soon...

## Mappings

Coming soon...

## Engines 

Coming soon...

<!-- - specify schemas either in memory or through URI -->


