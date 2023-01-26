# FAQ

## General

### Do you prescribe a certain ontology?
No. There's no ontology that suits all use-cases. This is why SKL schemas are customizable. You can choose what Nouns you use, what properties those Nouns have, and the names, parameters, and return values of Verbs. These customizations work as long as you update the Mappings between components. 

That said, we plan to provide a library of Schemas which solve most people's use-cases. Imagine being able to integrate with the [schema.org](https://schema.org) ontology and through it be able to access a bunch of services who's data can be translated into what schema.org describes. For example, you can connect to [https://schema.org/Event](https://schema.org/Event) and leverage all of the Mappings from [https://schema.org/Event](https://schema.org/Event) to data sources with Events, such as Ticketmaster, Stubhub, Eventbrite, etc. In this way, Schema.org can become a Unified API for various related services. Similarly, you could connect to healthcare APIs using the [FHIR](https://www.hl7.org/fhir/overview.html) ontology, or to several air-travel related data sources through the [NASA Air Traffic Management Ontology]([https://data.nasa.gov/ontologies/atmonto](https://data.nasa.gov/ontologies/atmonto)).

As more APIs, services, and components are mapped to popular ontologies, they start to establish themselves as standards within certain domain areas. You don't have to create a new ontology if you don't want to, simply use what's out there. For special cases or subjects/industries which we have not covered, anyone using SKL can create their own Schemas using their ontology of choice. We hope that you contribute any Schemas you create back to the public domain for other developers to use. If you need help customizing Schemas, or would like to consult with us on an implementation, please reach out to us on [Discord](https://discord.gg/stvfSB8kpG?ref=https://github.com/comake/skl-examples).

<!-- ### Why not just use code? -->

### SaaS systems aren't going to cooperate because their incentive is to differentiate. How do you get around that?
In some domain areas, API formats are standardized. For example, in healthcare, APIs are required by legislation to conform to a standard (eg. patient data APIs). In other areas, software purchasers (particularly larger customers) have significant influence on ensuring their software vendors provide APIs to access data. There are increasing market pressures against software vendors that force lock-in through lack of data access. Despite these examples of conformity, trying to convince software vendors to "standardize" their APIs is never going to work. The only way to create unified interfaces to various tools is externally. That's why SKL uses Mappings to translate data and capabilities to and from the formats used by APIs of different data sources. This approach does not require any changes to the APIs.

### How do you resolve differences between APIs when creating abstractions?
This is certainly the challenge in creating abstractions, but we think putting effort into figuring it out produces outcomes that far outweight having disconnected data or having to build lots of unscalable integrations through code. We've dealt with this in several instances:
-   Permissions for files stored in Google Drive are different than permissions for files stored in Dropbox
-   Categories for events in Ticketmaster are different than in Stubhub
-   Metrics about views on Facebook are quantified totally differently than views on YouTube

In each of these cases we created representations of the data that supported these variations without any serious limitations to capabilities. While we acknowledge that there is no such thing as a perfect abstraction, there are plenty of use cases for "good enough" abstractions as demonstrated by other unified API companies who use a fixed common data model. The problem with these other unified API services is that you are not in control. You have to pipe your data through someone else's systems, you are unable to change the abstraction should you want to, you are unable to add new integrations, etc.


## Comparison with other platforms and frameworks

### What about the Data Transfer Project?
The [Data Transfer Project](https://datatransferproject.dev/) (DTP) is a great initiative to increase data portability, interoperability, and end user control. It makes it easier for individuals to choose among services which facilitates competition, empowers individuals to try new services, and enables them to choose the offering that best suits their needs. Its major contributors include Apple, Facebook, Google, Microsoft, Twitter, and SmugMug. Despite its beneficial objectives and support from many large corporations, it does not enable the same level of interoperability, modularity, and customizability as SKL. Specifically.
- In DTP, schemas for common data models, which data from integrated services gets mapped to and from, are set by the creators of the project. They cannot be extended and edited by individual users. In SKL, schemas are variable, each user can customize their configuration.
- DTP data models and mappings are constructed entirely in Java code. Thus, it cannot be used with any programming language. SKL schemas and mappings are defined declaratively in JSON, a lightweight data-interchange format that has native support in every major programming language.
- When using DTP, data stays in control of apps. Although it's easier to port data between apps, DTP is not built to have users move their data to a personal data store.
- DTP specifies how to bulk import and export to and from services, it does not offer support for performing single capabilities or operations at a time (eg. sharing a file, sending an email, etc.).

### There are already tons of SDK generators for OpenAPI, why shouldn't I just use one of them? (eg. [https://openapi.tools/#sdk](https://openapi.tools/#sdk))
As far as we know, none work dynamically at runtime like [Standard SDK](https://www.comake.io/skl/sdk) (plus typescript types at development time). All the others you run a command to generate the SDK as code files before hand, which you then import in your code. You have to regenerate the SDK if the OpenAPI spec changes. Also, as far as we know, there are none that can or that plan to facilitate connections to other API specification in addition to REST such as GraphQL, OpenRPC, AsyncAPI, etc. This enables SKL to serve as a "unified api" to access multiple  data sources of different types in your own code base without having to route data through some 3rd-party service.