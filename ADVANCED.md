# Advanced Usage of Extensions

## User Data
Extensions can ask for user data during the time of installation of it. The user can either provide the data requested by extension (and therefore install it) or not provide the data (and not install it).

**Extensions should not collect PII (Personally Identifiable Information). Users can report if an extension tries to collect the data that should not be collected. Extensions can be suspended or permanently disabled from CineNexa, if they are found to do so.**

#### Workflow
- User clicks on "Install" on an extension from Extension Store
- User is redirected to a page, presenting all the information requested by the extension as a form
- User provides all the required data and submit
- The provided data is stored locally on the user's device and sent with each request to the specific extension


#### Enable User Data
Extensions needs to provide the url of their configuration.json file while publishing the extension on CineNexa. This file builds the form which is presented to the user.

The input types available are:
- checkbox - Multiple selectable values
- radio - Single selectable value
- text - input text
- dropdown - dropdown menu 

As a rule of thumb, checkbox should be used for the fields for which more than one option can be selected, radio for fields which only require single selected value and dropdown for the fields which require single selected value and have a long list of selectable values.

A sample of manifest.json:
```json
[
  {
    "id" : "q-genre",
    "type" : "checkbox",
    "title" : "Genres",
    "description" : "Select the genres",
    "fields" : [
      "Action",
      "Adventure",
      "Comedy",
      "Drama"
    ],
    "required" : true
  },
  {
    "id" : "q-country",
    "type" : "dropdown",
    "title" : "Country",
    "description" : "Select the countries",
    "fields" : [
      "USA",
      "UK",
      "Germany",
      "France"
    ],
    "required" : false
  },
  {
    "id" : "q-lang",
    "type" : "radio",
    "title" : "Language",
    "description" : "Select the language",
    "fields" : [
      "English",
      "German",
      "Hindi",
      "Arabic"
    ],
    "required" : true
  },
  {
    "id" : "q-key",
    "type" : "text",
    "title" : "API key",
    "description" : "Enter your api key",
    "required" : true,
    "maxLines" : 4,
    "inputType" : "string"  //types - string (alphanumeric) & number (numeric)
  }
]
```

**All fields are mandatory except "maxLines" & "inputType" in text.** 

#### Accesing User Data
Normally, a GET call is performed to an extension's endpoint and all the data related to the movie/show is url-encoded.

When an extension marks itself as an extension which requires user data (by providing manifest.json url during publishing process), PUT calls are performed for that extension. The user data is passed in the body of PUT request but any data related to the movie/show is still url-encoded and can be accessed normally from query params.

The user provided data is url-encoded and can be accessed from the query params:
```js
let queryParams = req.query;
let encodedData = req.body;

let userData = JSON.parse(encodedData.params);

queryParams.name; // Name of the requested movie/show
encodedData['q-genre']; // Genre selected by user in the example above
```
