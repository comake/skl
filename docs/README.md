# Standard Knowledge Language

Standard Knowledge Language (SKL) is an open-source framework for building composable software and enabling interoperability. It is a methodology for how software can connect to data and other software components through customizable abstractions called schemas.

The following pages provide a practical introduction for developers who want to:

- [use schemas](./schemas.md) to build software integrations using SKL
- [use an engine](./engines.md) to execute schemas from code
- [build schemas](./build-schemas.md) and make them available for others to use
- [build an engine](./build-an-engine.md) in your language of choice

If you have any questions, please [reach out to us on Discord](https://discord.gg/stvfSB8kpG?ref=https://github.com/comake/skl-examples) or [open or contribute to a discussion](https://github.com/comake/skl/discussions).

## Motivations

Today, people lack clarity because each app they work with only shows them a partial view of their work. Trying to offer a holistic and integrated experience is hard and results in a messy web of one-to-one integrations that is hard to maintain. This is true both for companies trying to consolidate data between tools they use internally, and for software companies wanting to let their users push or pull data to or from other software tools.

Developers commonly use SDKs or other code packages (node modules, ruby gems, etc.) to interact with the API of a software tool they want to integrate into their application. These methods provide convenience over having to writing code to communicate directly using HTTP requests but still have limitations. Due to resource constraints, software tools may not have an SDK or code package targeting every programming language or execution environment a developer may be programming their application in. Even if they find suitable wrappers for the API they wish to interact with, developers are still left with the task of reading API documentation to know how to translate the unique data formats and capabilities of each API to the data formats and features of their application. This is not a trivial task, especially as the number of integrations they need increases.

The data integration problems outlined above are not new. Over a decade ago, Tim Berners Lee (colloquially known as the founder of the internet), proposed the concept of Linked Data to mitigate data integration issues. Linked Data was to provide a format that is both machine and human readable, allowing different software tools and their creators to have a common understanding of data and what it represents. Unfortunately, it did not gain as strong of a following by tech companies as many would have hoped and is not used widely today.

There are still proponents who call upon tech companies to convert their APIs or build Linked Data compatible versions of their APIs. There are also proponents for establishing standard API features that solve for common capabilities across APIs so that software tools and developers can more easily access similar features by attributes of the feature rather than of the API.

The success of proposed methods is dependent on developers at different companies collectively agreeing to architect their APIs according to a shared criteria that doesn’t yet exist and that could limit their innovation or development cycles. In the fast-moving world of software startups, convincing enough companies to invest in the collective establishment of these standards (sometimes in concert with their competitors) is hard to do. Large companies are also at times loath to convert to more standard data formats, ontologies, and APIs because they amass great value by controlling the flow of data through and within their ecosystems, in part through proprietary APIs. The silos of data that arise through competing technology ecosystems ultimately lead to vendor lock-in that can stifle users’ range of capabilities and broader software innovation.

There is no perfect ontology for all use cases, and part of the value of Linked Data is that anyone can easily create their own or contribute to existing ontologies. For those software tools that do choose to invest in building Linked Data APIs, this independent proliferation of various (and sometimes competing) ontologies of Linked Data can make it so that there is still a lot of manual work that has to be done by humans to integrate data which is formatted according to different ontologies. An important missing piece is therefore the standard translation across various ontologies, representations of data, and software capabilities.

Standard Knowledge Language solves these problems by defining a protocol for interacting with non-standard data formats and APIs through standard abstractions of data and capabilities. Unlike the approaches described above, SKL does not require existing API providers to change their APIs or offerings. Instead, it provides a streamlined way for developers to easily connect to a theoretically infinite number of APIs simultaneously.