# Terminology

### Noun
An abstraction of a type of data structure used by the APIs of software tools (eg. File, Person, Task, Patient, etc.). Many software tools use data structures with the same name but with slight differences. For example, Google Drive and Dropbox both have a data structure called “File” but don’t use the same fields to represent files. Nouns create a shared middle ground which developers can use instead of writing code to interact with each software tool’s unique data structures. Nouns are extendable and customizable, allowing developers to add fields for their unique needs. A Noun can have fields that are not supported by every tool that has mappings to it. For example, a File on a hard drive might not have sharing permissions, whereas a File on OneDrive will. Both can be mapped to the standard File Noun.

### Verb
A representation of a capability exposed by a software tool (eg. Share, Send, Download, Like, etc.). Like their unique data structures, software tools expose similar capabilities that vary slightly in their inputs, outputs, and execution methods, even though they may have the same meaning or eventual effect. Verbs create a simplified way through which developers can use the capabilities of software tools without having to know the distinct requirements of each. Verbs may be configured to be run at a specific time, upon a specific schedule, or in response to specific events, for example in response to an event from a webhook registered with the API of an Integration. Verbs can also be composed together to form larger processes.

### Mapping
A configuration which specifies how how a Noun can be translated to or from a unique data format specific to an Integration, or how a Verb can be translated to or from a unique capability of a software tool.

### Entity
an instance of data conforming to a Noun or Verb schema, often corresponding to a thing or capability in the real world.

### Schema
A set of configurations prescribing the shape that instances of a Noun must adhere to or detailing what capabilities are possible and how those capabilities are performed. All schemas within SKL are either a type of Noun or Verb. Schemas can inherit from other schemas. SKL uses [the SHACL ontology](https://www.w3.org/TR/shacl/) to define shapes.

### Integration
A software tool which has data and capabilities accessible via API that SKL can communicate with to query for, mutate, receive messages about, or otherwise access or perform actions over data.

### Standard Knowledge Application
A software application which downloads, bundles, or otherwise accesses SKL schemas, configuration files, and/or code and uses them to interact with Integrations and/or Interface Components by mapping the unique data structures and capabilities of those Integrations or Interfaces in and out of standard Noun and Verb schemas according to the SKL protocol.

### Standard Knowledge Data Store (SKDS)
A type of Integration that stores instances of Nouns, Verbs, and Mappings and/or other SKL logic on behalf of a developer or end user.

### SKL Engine
A library of code that can read SKL Schemas and execute Verbs and Mappings. An Engine's main requirements are that developers using it can set a source of schemas and call Verbs. (**Note**: SKL Engines used to be called Standard Knowledge Query Language (SKQL) Engines.)

### Operation

