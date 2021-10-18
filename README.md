# Unit21's API description and Postman collection

Public mirror of Unit21's API documentation.

Unit21's API documentation uses the openAPI specification.
The API reference is hosted on [an interactive website](https://docs.unit21.ai/developers/reference),
where you can explore calls to different endpoints. 

As the OAS spec is very portable, Unit21 has made a copy of it available in this repo.
For most customers, the interactive site is the best way to explore the documentation.
However, some customers might find it useful to have a spec on hand, to do things like create a mock server, or simply explore the API using a different parser.

There is a Postman collection available, which includes a calls that cannot be tested on the site (e.g. bulk event creation).

## Explore the API using openapi.yaml

1. Download the file at [openapi.yaml](openapi.yaml).

2. Run `openapi preview-docs openapi.yaml`.

3. Open the local server in your browser.

## Explore the API using the Postman collection

1. Download [the collection in this repo](Unit21_internal_API.postman_collection.json).

2. In Postman, select **Import** and import the collection JSON file (in v2.1 format).

3. Configure [collection variables](https://learning.postman.com/docs/postman/variables-and-environments/variables/#defining-variables-in-scripts). Click on options for the imported collection and select "Edit".

![In the sidebar, click on edit](edit-variables.png)

4. Populate the collection variables:
  * `host` with your sandbox environment, for example `sandbox2-api.unit21.com` or `sandbox1-api.unit21.com`.
  * `protocol` should be `https`.
  * `u21-key` with the [API developer key](https://docs.unit21.ai/developers/docs/api-keys).
  * `orgName` with your organization name (passed to you by a Unit21 team member), for example `unit_21`.

### Notes:

- To run a new test that avoids ID collisions, change the `randId` collection variable to a new value.
  For more convenience, you could create a collection-level script that automatically sets `randId` to be a random string upon running the first request in the collection e.g.

```jsx
if (pm.info.requestName == 'create userA') {
    var uuid = require('uuid')
    randId = uuid();
    pm.collectionVariables.set('randId', randId);
}
```

- If you encounter `423 LOCKED` responses from the API on **update** or **get** API requests,
  the requested object is still being processed. Retrying the request after a delay should result in a successful request.
- The `(OPTIONAL) Sleep 2 S` requests are included in the collection
  to avoid `424 LOCKED` responses when running the collection requests in quick succession i.e. using Postman Collection Runner.
