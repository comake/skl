# Nouns

A Noun is a schema for a data structure representing a concept used by software tools. When you use SKL, everything is translated through a Noun: emails, notes, webpages, files, podcast episodes, news articles, receipts, prescriptions, medications, locations, etc.

Many software tools use data structures with the same name but with slight differences. Nouns are an abstraction of these different data structures that enables a higher degree of interoperability between them.

**Example**

The developers of [Google Drive](https://www.google.com/drive/), [Dropbox](https://www.dropbox.com/), and [Box](https://www.box.com/) each defined a different data structure used for representing files in their respective system. Normally, when integrating files from these three tools into an application, a developer would have to write different code to work with each. Instead, using SKL, the unique data structures of each tool can be translated (see [Mappings](https://github.com/comake/skl/blob/main/fundamentals/README.md#mappings) for info about the translation process) into instances of the File Noun. The resulting File entities from each system include common fields like name, mime type, and size. They may also include fields that only some systems support such as a unique hash of the contents of the file, which Box and Dropbox support but Google Drive doesn't. Dropbox also has support for adding any arbitrary key/value data to files, this information will be included in any File entities from Dropbox.

In this way, Nouns are abstractions of the commonalities between each Integration’s representation of a concept but are also extendable and customizable, allowing the unique fields each tool has to still exist on entities.

**Entity**

An entity is an instance of a Noun which conforms to the Noun's schema.

**Properties**

A Property of a Noun is a characteristic that has a label and a value, such as the `title` property of a File, or the `status` property of a Task. Each property has a schema that can defines its name, description, type, default value, range, cardinality, length or pattern. Properties of nouns can be required (having a minimim cardinality of one).

**SHACL**

The schema that entities of a Noun must conform to is defined using the [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/). You can see an example of the File noun as a SHACL shape in the [SKL Dictionary](https://github.com/comake/skl-dictionary/blob/main/nouns/file/schema.json).

{% hint style="success" %}
Most Noun schemas have the `shacl:closed` property set to `false`. This means that data conforming to the schema can include any arbitrary properties not explicitly enumerated as a property in the schema.
{% endhint %}

{% hint style="info" %}
We acknowledge that it's impossible to create a single ontology that can appease every use case. In addition, it's a huge undertaking to attempt to model an abstraction for every data structure a software tool may need to work with. In light of these facts, SKL Schemas are highly modular, customizable, and are evaluated at runtime to enable any capabilities an application or end user may need. In addition, we are continuously adding to the \[SKL Dictionary]\(https://github.com/comake/skl-dictionary), an open source Library of Nouns, Verbs, and Mappings which developers can use and contribute to.
{% endhint %}
