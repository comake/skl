# Use Cases



## Universal File Browser

A developer builds an application which displays to users all their folders and files from any file storage tool they connect (eg. Dropbox, Google Drive, OneDrive, etc.). The application contains code which uses SKL to recursively request and copy the metadata of all files and folders within a user's accounts regardless of what service they exist on so that it may display and index them. This application would work with the `File` and `Folder` Nouns, Verbs like `getFilesInFolder`, `Move`, `Copy`, and `Download`, and use Mappings that translate between those Verbs and each file storage tool's API. The developer can offer options to Move and Copy files between services or download a file regardless of where it's stored.

When the developer wants to support an additional integration, all he/she needs to do is add additional Mappings between the standard Verbs their application uses and the new Integration. Since these Mappings are configuration and not code, there is no need to change the programming of the universal file browser Application in order to support the new Integration. Since this file browser is made to represent and interact with the standard `Files` Noun, it could also easily represent other `Files` that might come from different types of Integrations, such as files attached to or referenced in email and messaging tools (e.g. Gmail, Slack, etc.) and files that are attached in project management tools (e.g, Asana, Monday.com, etc.).

## Centralizing Customer information

A marketing team wants to see consolidated profile information about each of their customers but the data is spread across multiple software tools. They can use SKL to map each software tool's representation of their customers to the standard `Customer` Noun, matching data on known identifiers such as email address, and phone number to create a more complete picture about each customer.

## Centralizing Health Information

A healthcare patient wants to build up and maintain his/her own consolidated electronic health record (EHR) with data from multiple providers in a simple, secure, and decentralized way. This person may want to:
- easily compile a comprehensive historical digital record of his or her health at any time
- have control of his or her health data by storing it in a dedicated and isolated database in the cloud or on their own device and having the ability to change storage providers at any time
- manage and easily provide or revoke 3rd party access to data in their personal EHR (e.g., to a provider, family member, pharmaceutical company, etc.)

Integrating thier data using SKL would enable the individual to avoid having to refill similar forms every time he or she visits a doctor or other healthcare provider. It would also empower a provider to offer more holistic healthcare by having a more complete view of the patient’s health history rather than only being able to view the data derived from interactions that patient had with that provider.

SKL can work as the glue that connects health-tech services across various providers in order allow the consolidation of health data. It can enable translations between data and a variety of platforms like those that use the FHIR data standard `Patient`. Furthermore, if existing platforms have software integration, data processing, or other useful services that a developer or user wants to leverage, they could be mapped to the corresponding SKL Nouns and Verbs in order to make those services accessible in a modular way.

## Unified API for Warehouse Management Systems
A company that builds warehouse automation robots wants to integrate with a variety of warehouse management systems. Rather than building a series of one-to-one integrations between their software system and the various warehouse management systems, they can establish or use existing SKL Mappings to translate between the various third party warehouse management systems and the desired Nouns and Verbs (e.g., `Order`, `Product`, `Pick`, etc.).
