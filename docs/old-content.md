This diagram gives more detail about the procedure SKQL takes when a Verb is executed:

![](../images/verb.jpg)

Take for example a Verb called `getFilesInFolder`. To use this Verb to get all the files in a folder stored on Dropbox the following function is used (written in javascript):
```javascript
const files = await SKQL.getFilesInFolder({ account, folder });
```
where `account` and `folder` are objects conforming to the Account and Folder Nouns respectively. SKQL then follows these steps to execute the Verb:

1. Find the configuration for the Verb `getFilesInFolder` from the current schema source. Also find the Mappings which translate between the `getFilesInFolder` Verb and the Dropbox Integration.
2. Use the Mappings to translate the Verb inputs into a web request to send to the Dropbox API. In this case, the Mappings will translate the `account` and `folder` inputs into the `https://api.dropboxapi.com/2/files/list_folder` endpoint with a `path` query parameter set to the input folder's identifier in Dropbox.
3. Send the web request and get the response.
4. Use the Mappings to translate the raw web request response into the standard Verb output, a collection of File Noun entities.
5. Return the standard Verb output.
