# Nouns

## What is a Noun?

A Noun is a schema for a data structure representing a concept used by Integrations. When you use SKL, everything is translated through a Noun: emails, notes, webpages, files, podcast episodes, news articles, receipts, prescriptions, medications, locations, etc.

## Why do we need Nouns?

Many software tools use data structures with the same name but with slight differences. Nouns are an abstraction of these different data structures that enables a higher degree of interoperability between them. Nouns make is so that data can be understood by what it represents rather than relying on where it comes from or where it is stored.

## Example

The developers of [Google Drive](https://www.google.com/drive/), [Dropbox](https://www.dropbox.com/), and [Box](https://www.box.com/) each defined a different data structure to represent files in their respective software and APIs. Normally, when integrating files from these tools into an application, a developer would have to write different code to work with each. 

Instead, using SKL, the unique data structures of each tool can be translated (via [Mappings](./mappings.md)) into instances of the File Noun. The resulting File entities from each system include common properties like name, mime type, and size. They may also include properties that only some systems support such as a unique hash of the contents of the file, which Box and Dropbox support but Google Drive doesn't. Dropbox also has support for adding any arbitrary key/value data to files, this information will be included in any File entities from Dropbox.

In this way, Nouns are abstractions of the commonalities between each Integration’s representation of a concept but are also extendable and customizable, allowing the unique properties each tool has to still exist on entities.

{% hint style="info" %}
We acknowledge that it's impossible to create a single ontology that can appease every use case. In addition, it's a huge undertaking to attempt to model an abstraction for every data structure a software tool may need to work with. In light of these facts, SKL is highly modular, customizable, and it's components are evaluated at runtime to enable any capabilities an application or end user may need. In addition, we are continuously adding to the \[SKL Dictionary]\(https://github.com/comake/skl-dictionary), an open source Library of Nouns, Verbs, and Mappings which developers can use and contribute to.
{% endhint %}

## Entities

An entity is an instance of a Noun which conforms to the Noun's schema.

## Properties

A Property of a Noun is a characteristic that has a label and a value, such as the `title` property of a File, or the `status` property of a Task. Each property has a schema that can define its name, description, type, default value, range, cardinality, length or pattern. Properties of nouns can be required (having a minimim cardinality of one).