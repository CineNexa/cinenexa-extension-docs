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

A sample of manifest.json:
```json
{
    "title" : "Genre",
    "description" : "Select your favorite genres",
    "type" : "checkbox",
    "fields" : [
        "Action",
        "Adventure",
        "Animaiton"...
    ],
    "is_mandatory" : true,
}
```

#### Accesing User Data
The user provided data is url-encoded and can be accessed from the query params:
```js
req.query.params
```
