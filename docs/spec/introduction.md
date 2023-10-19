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
| dcterms: | http://purl.org/dc/elements/1.1/ |
| rr: | http://www.w3.org/ns/r2rml# |
| rml: | http://semweb.mmlab.be/ns/rml# |
| owl: | http://www.w3.org/2002/07/owl# |
| xsd: | http://www.w3.org/2001/XMLSchema# |

## SKL Ontology

The core SKL Ontology namespace is https://standardknowledge.com/ontologies/core/

```
https://standardknowledge.com/ontologies/core/
```

The core SKL Ontology preferred prefix is `skl`.

## Schemas

A project which uses SKL contains or accesses a collection of SKL Schemas which define the data formats and capabilities the project uses.

SKL Schemas are modeled using the Resouce Description Framework ([RDF](https://en.wikipedia.org/wiki/Resource_Description_Framework)), and are most commonly viewed and edited in the [JSON-LD](https://json-ld.org/) format. As RDF, Schemas are identified by and relate to one another via URI. This forms a graph of the data formats and capabilities a project uses.

The SKL Ontology consists of SKL specific classes, some of which use other ontologies and specificiations such as [RML](https://rml.io/specs/rml/), [SHACL](https://www.w3.org/TR/shacl/), and [OpenAPI](https://spec.openapis.org/oas/v3.1.0)


## Nouns

A Noun Schema is an RDF Node which defines a data structure representing a concept relevant to a project or application. It can be used by an [Engine](#engines) to validate entities or to generate data. A Noun consists of the following properties:

| name | predicate | description |
| ---- | --------- | ----------- |
| label | `rdfs:label` | **REQUIRED**. A human readable identifier for the Noun. |
| description | `dcterms:description` | A semantic description what the Noun represents. |
| properties | `shacl:property` | Specification of properties that entities of the noun MUST conform to. |
| closed | `shacl:closed` | Whether entities of the Noun can have properties other than the ones explicitly defined in properties. |
| context | `skl:context` | A JSON-LD Context Definition used to compact entities of this noun into a more human readable JSON format. |

As seen in the table above, SKL Nouns use the [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/) to define their properties. Every Noun is a SHACL NodeShape.



## Engines 

Coming soon...
<!-- - specify schemas either in memory or through URI -->


