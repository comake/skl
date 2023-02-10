# FAQs

## General

### Do you prescribe a certain ontology?

No. There's no ontology that suits all use-cases. This is why SKL schemas are customizable. You can choose what Nouns you use, what properties those Nouns have, and the names, parameters, and return values of Verbs. These customizations work as long as you update the Mappings between components.

That said, we plan to provide a library of Schemas which solve most people's use-cases. Imagine being able to integrate with the [schema.org](https://schema.org) ontology and through it be able to access a bunch of services who's data can be translated into what schema.org describes. For example, you can connect to [schema.org/Event](https://schema.org/Event) and leverage all of the Mappings relating [schema.org/Event](https://schema.org/Event) to data sources with Events, such as Ticketmaster, Stubhub, Eventbrite, etc. In this way, Schema.org can become a Unified API for various related services. Similarly, you could connect to healthcare APIs using the [FHIR](https://www.hl7.org/fhir/overview.html) ontology, or to several air-travel related data sources through the [NASA Air Traffic Management](https://data.nasa.gov/ontologies/atmonto) ontology.

As more APIs, services, and components are mapped to popular ontologies, they may establish themselves as standards within certain domain areas. You don't have to create a new ontology if you don't want to, simply use what's out there. For special cases or subjects/industries which we have not covered, anyone using SKL can create their own Schemas using their ontology of choice. We hope that you contribute any Schemas you create back to the public domain for other developers to use. If you need help customizing Schemas, or would like to consult with us on an implementation, please reach out to us on [Discord](https://discord.gg/stvfSB8kpG?ref=https://github.com/comake/skl-examples).

### Why not just use code?

The benefit of facilitating integrations through schema rather than code is to encourage use across languages. For example, if someone creates Mappings in JSON between [schema.org/FlightReservation](https://schema.org/FlightReservation) and several different APIs for various airlines, then those same Mappings can be used by a SKL Engine in any language (JavaScript, Ruby, Rust, etc.). If this were to be done in code, you would need to do the same work over in every programming language. Using the same configuration logic across languages also makes it more accessible to different developer perspectives and easier to maintain. We plan to build tooling (e.g., a no-code editor, AI tools) to make working with Schemas as simple as possible.

### Software companies rarely cooperate on creating standardized features across their APIs. How can SKL get around that?

Many different unified API companies have already proven that creating common models around similar APIs is possible and that doing so can generate significant value for developers. Moreover, API formats are standardized in some domain areas by law. For example, in healthcare, patient access APIs are required to conform to a domain-specific [standard](https://www.cms.gov/regulations-and-guidance/guidance/interoperability/index). In other areas, software purchasers (particularly larger customers) have significant influence on ensuring their software vendors provide APIs to access data. Those software purchasers are able to use SKL independently to create abstractions over their vendors’ APIs as a way to standardize the data. This is possible through the use of SKL Mappings which translate data and capabilities to and from the formats used by APIs of different data sources. This approach does not require any changes to the APIs. Trying to convince software vendors to "standardize" their APIs is never going to work, and may even stifle innovation in some cases. We believe the best way to create unified interfaces to various tools is externally.

### Why should I use SKL instead of a Unified API?

Beyond not having to pay per query, using SKL puts you fully in control of which data sources are integrated and how they are mapped to a common model. If you would like to add a certain integration that a unified API doesn’t offer, you are able to easily add it yourself. Similarly, if a common model or abstraction doesn’t solve all of your needs you are able to easily edit the abstraction, or query the source directly and receive raw responses without going through the abstraction.

There are also certain security and compliance considerations. You or your company may want to avoiding vendor lock-in or may prefer to keep all of your data on your own infrastructure.

Because SKL is open-source, modular, and highly-composable, you can re-use schemas and mappings across projects and programming languages. You can find pre-built schemas in the [SKL Dictionary](https://github.com/comake/skl-dictionary). We are adding more on a weekly basis. We'd love for you to contribute any schemas you create too!

### How do you resolve differences between APIs when creating abstractions?

This is certainly the challenge in creating abstractions, but we think putting effort into figuring it out produces outcomes that far outweight having disconnected data or having to build lots of unscalable integrations through code. We've dealt with this in several instances:

* Permissions for files stored in Google Drive are different than permissions for files stored in Dropbox
* Categories for events in Ticketmaster are different than in Stubhub
* Metrics about views on Facebook are quantified differently than views on YouTube

Although the data in these examples can't always be compared directly (e.g., "can view" in Dropbox vs. "can view" in Google Drive), we have found ways of successfully creating unified representations of the data that support differentiations while maintaining commonalities. While we acknowledge that there is no such thing as a perfect abstraction, there are plenty of use cases for "good enough" abstractions as demonstrated by other unified API companies who use a fixed common data model. The problem with these other unified API services is that you are not in control. You have to pipe your data through someone else's systems, you are unable to change the abstraction if you want to, and you are unable to add new integrations. If you want help to create your own abstractions through SKL Schemas or to add an integration into your Schemas, please reach out to us on [Discord](https://discord.gg/stvfSB8kpG?ref=https://github.com/comake/skl-examples).

## Comparison with other platforms & frameworks

### How is this different from GraphQL?

GraphQL, like SKL, is powered by schemas which define the data formats and capabilities (queries & mutations) of an API. With it, a developer can create any API they want as a wrapper around data sources (eg., [databases](https://www.zdnet.com/article/graphql-for-databases-a-layer-for-universal-database-access/), files, other APIs, [even the DOM](https://youtu.be/Xi3sxygtDc4?t=848)). However, this is where GraphQL stops, it defines the interface for interaction with the system, but not how data is actually retrieved or modified when the interface is used. A developer creating a GraphQL server has to write in code all the [`resolvers`](https://graphql.org/learn/execution/#root-fields-resolvers) to translate between the data formats and capabilities exposed by the GraphQL API and the underlying data sources. This is exactly the problem that SKL solves! SKL Mappings define rules for how one data format (Noun) gets translated into another, and how the name and parameters of an operation (Verb), like a GraphQL query or mutation, get translated into an operation in a data source, and how the response from the data source gets translated back into the response of the operation. Developers creating GraphQL servers can use SKL Schemas to encode what happens in the resolver of each GraphQL query and mutation, thereby, they will not have to write custom code for every interaction one of their GraphQL resolvers has with a data source.

### What about the Data Transfer Project?

The [Data Transfer Project](https://datatransferproject.dev/) (DTP) is a great initiative to increase data portability, interoperability, and end user control. It makes it easier for individuals to choose among services which facilitates competition, empowers individuals to try new services, and enables them to choose the offering that best suits their needs. Its major contributors include Apple, Facebook, Google, Microsoft, Twitter, and SmugMug. Despite its beneficial objectives and support from many large corporations, it does not enable the same level of interoperability, modularity, and customizability as SKL. Specifically:

* In DTP, schemas for common data models, which data from integrated services gets mapped to and from, are set by the creators of the project. They cannot be extended and edited by individual users unless you fork, modify, and rebuilt the project. In SKL, schemas are variable; each user can customize their configuration.
* DTP data models and mappings are constructed entirely in Java code. Thus, it cannot be used with any programming language. SKL schemas and mappings are defined declaratively in JSON, a lightweight data-interchange format that has native support in every major programming language.
* When using DTP, data stays in control of apps. Although it's easier to port data between apps, DTP is not built to have users move their data to a personal data store.
* DTP specifies how to bulk import and export to and from services, it does not offer support for performing single capabilities or operations at a time (eg. sharing a file, sending an email, etc.).

### There are already tons of SDK generators for OpenAPI, why shouldn't I just use one of them? (eg. [https://openapi.tools/#sdk](https://openapi.tools/#sdk))

As far as we know, none work dynamically at runtime like [Standard SDK](https://www.comake.io/skl/sdk) (plus typescript types in your IDE). When using these SDK generators, you run a command to generate the SDK as code files before hand, which you then import in your code. You have to regenerate the SDK if the OpenAPI spec changes. Also, as far as we know, there are none that can or that plan to facilitate connections to other API specification in addition to REST such as GraphQL, OpenRPC, AsyncAPI, etc. This enables SKL to serve as a "unified api" to access multiple data sources of different types in your own code base without having to route data through some 3rd-party service.
