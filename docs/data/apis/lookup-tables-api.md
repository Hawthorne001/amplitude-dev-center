---
title: Lookup Table API
description: The Lookup Table API lets you create, get, update, and delete lookup tables to augment your data.
---

Lookup tables let you augment user and event properties. Instead of using formulas, you can upload a CSV file that contains property mappings to derive new properties. 

To create a lookup property, create a Lookup Table to reference. You can retrieve and update each of the tables using the API. Lookup Tables have unique names and have a project scope.

You can also create and manage lookup tables from the Amplitude web app. See [Lookup Table](/data/sources/lookup-table) for more information.

--8<-- "includes/postman.md"

## Endpoints

| Region | Endpoint |
| --- | --- |
| Standard Server | https://data-api.amplitude.com/api/3/lookup_table |
| EU Residency Server | https://data-api.eu.amplitude.com/api/3/lookup_table |

--8<-- "includes/auth-basic.md"

## Considerations

The CSV file must follow the following requirements:

- The max file size is 100 MB and the file can't have more than 1,000,000 rows.
- The first row must contain column names/headers.
- The first column must correspond to the mapping property value and must contain *unique* values. Lookup Tables search for exact matches, and are *case-sensitive*.
- Columns are separated by commas.
- Rows are separated by line breaks.
- If a field value contains commas or quotes, it should be wrapped within double quotation marks. The first double quote signifies the beginning of the column data, and the last double quote marks the end. If the value contains a string with double quotes, these are replaced by double quotes.

## Create a Lookup Table

Create a Lookup Table object by uploading a CSV that maps an existing property to the new properties to create. Send the request with the type multipart/form-data type.

### Parameters

|<div class="big-column">Name</div>| Required| Type| Description|
|-----|------|------|-----|
|`name`     | <span class="required">Required</span>| String| Name of the table.|
|`file`     | <span class="required">Required</span>| File| A CSV representation of the mappings.|
|`key`      | <span class="required">Required</span>| String| Column in CSV to use as key of lookup table.|
|`property` | <span class="required">Required</span>| JSON| Property in Amplitude to map to the key column in CSV.|
|`property.value` | <span class="required">Required</span>| String| Name of property in Amplitude.|
|`property.type` | <span class="required">Required</span>| String| Type of property in Amplitude.|
|`property.groupType` | |String| Required only if property is a group property.|

### Example request

=== "cURL"

    ```curl
    curl -L -X POST 'https://data-api.amplitude.com/api/3/lookup_table' \
         -u API_KEY:SECRET_KEY \
         -F 'file=@"/path/to/file.csv"' \
         -F 'name=":name"' \
         -F 'key=":key"' \
         -F 'property="{\"value\": \":propertyName\", \"type\": \":propertyType\", \"groupType\":  \":propertyGroupType\"}";type=application/json'
    ```

=== "HTTP"

    ```bash
    POST '/api/3/lookup_table' HTTP/1.1
    Host: data-api.amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

    ------WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="key"

    :key
    ------WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="name"

    :name
    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name=":name"; filename="file.csv"
    Content-Type: text/csv

    (data)
    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="property"
    Content-Type: application/json

    {"value": ":propertyName", "type": ":propertyType", "groupType": ":propertyGroupType"}
    ------WebKitFormBoundary7MA4YWxkTrZu0gW--
    
    ```

### Responses

=== "Success"

    ```json
    {
        "appId": "<projectId>",
        "name": "example-lookup",
        "columnHeaders": [
            "Language"
        ],
        "createdAt": 1715912516,
        "createdBy": "api",
        "lastModifiedAt": 1715912516,
        "lastModifiedBy": "api",
        "isDeleted": false,
        "isConfigured": true,
        "keyColumnHeader": "SKU",
        "keyProperty": {
            "type": "event",
            "value": "example",
            "groupType": "User"
        },
        "fileName": "lookup-table-example.csv",
        "rowCount": 3,
        "sizeBytes": 0,
    }
    ```

## Retrieve a Lookup Table

Retrieve a Lookup Table by its name.

### Parameters

|<div class="big-column">Name</div>| Required| Type| Description|
|-----|------|----|----|
|`name` | <span class="required">Required</span>| String| Name of the table.|

### Example request

=== "cURL"

    ```curl
    curl -L -X GET 'https://data-api.amplitude.com/api/3/lookup_table/:name' \
         -u API_KEY:SECRET_KEY
    ```

=== "HTTP"

    ```bash
    GET /api/3/lookup_table/:name HTTP/1.1
    Host: data-api.amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    ```

### Response

=== "Success"

    ```json
    {
        "appId": "<projectId>",
        "name": "example-lookup",
        "columnHeaders": [
            "Language"
        ],
        "createdAt": 1715912516,
        "createdBy": "api",
        "lastModifiedAt": 1715912516,
        "lastModifiedBy": "api",
        "isDeleted": false,
        "isConfigured": true,
        "keyColumnHeader": "SKU",
        "keyProperty": {
            "type": "event",
            "value": "example",
            "groupType": "User"
        },
        "fileName": "lookup-table-example.csv",
        "rowCount": 3,
        "sizeBytes": 5,
    }
    ```

## Download csv

Download the lookup table object as a CSV. Any incremental changes are applied in the downloaded file

### Parameters

|<div class="big-column">Name</div>| Required| Type| Description|
|-----|------|----|----|
|`name` | <span class="required">Required</span>| String| Name of the table.|

### Example request

=== "cURL"

    ```curl
    curl -L -X GET 'https://data-api.amplitude.com/api/3/lookup_table/:name/csv' \
         -u API_KEY:SECRET_KEY
    ```

=== "HTTP"

    ```bash
    GET /api/3/lookup_table/:name/csv HTTP/1.1
    Host: data-api.amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    ```

## Override a Lookup Table

Override a Lookup Table object by uploading a CSV that replaces the CSV already uploaded to Amplitude. Send the request with the type multipart/form-data type.

### Parameters

|<div class="big-column">Name</div>| Required| Type| Description|
|-----|------|------|-----|
|`name`     | <span class="required">Required</span>| String| Name of the table.|
|`file`     | | File| A CSV representation of the mappings.|
|`property` | | JSON| Property in Amplitude to map to the key column in CSV.|
|`property.value` | | String| Name of property in Amplitude.|
|`property.type` | | String| Type of property in Amplitude.|
|`property.groupType` | |String| Required only if property is a group property.|

### Example request

=== "cURL"

    ```curl
    curl -L -X PUT 'https://data-api.amplitude.com/api/3/lookup_table/:name' \
         -u API_KEY:SECRET_KEY \
         -F 'file=@"/path/to/file.csv"' \
         -F 'property="{\"value\": \":propertyName\", \"type\": \":propertyType\", \"groupType\":  \":propertyGroupType\"}";type=application/json'
    ```

=== "HTTP"

    ```bash
    PUT '/api/3/lookup_table/:name' HTTP/1.1
    Host: data-api.amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name=":name"; filename="file.csv"
    Content-Type: text/csv

    (data)
    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="property"
    Content-Type: application/json

    {"value": ":propertyName", "type": ":propertyType", "groupType": ":propertyGroupType"}
    ------WebKitFormBoundary7MA4YWxkTrZu0gW--
    
    ```

### Responses

=== "Success"

    ```json
    {
        "appId": "<projectId>",
        "name": "example-lookup",
        "columnHeaders": [
            "Language"
        ],
        "createdAt": 1715912516,
        "createdBy": "api",
        "lastModifiedAt": 1715912516,
        "lastModifiedBy": "api",
        "isDeleted": false,
        "isConfigured": true,
        "keyColumnHeader": "SKU",
        "keyProperty": {
            "type": "event",
            "value": "example",
            "groupType": "User"
        },
        "fileName": "lookup-table-example.csv",
        "rowCount": 3,
        "sizeBytes": 0,
    }
    ```

## Update a Lookup Table

Update a Lookup Table's columns and data. If a CSV file is provided, the file is merged with the existing CSV within Amplitude. This allows for incremental updates of the CSV, instead of a complete replacement.

### Parameters

|<div class="big-column">Name</div>| Required| Type| Description|
|-----|------|------|-----|
|`name`     | <span class="required">Required</span>| String| Name of the table.|
|`file`     | | File| A CSV representation of the mappings.|
|`property` | | JSON| Property in Amplitude to map to the key column in CSV.|
|`property.value` | | String| Name of property in Amplitude.|
|`property.type` | | String| Type of property in Amplitude.|
|`property.groupType` | |String| Required only if property is a group property.|

### Example request

=== "cURL"

    ```curl
    curl -L -X PATCH 'https://data-api.amplitude.com/api/3/lookup_table/:name' \
         -u API_KEY:SECRET_KEY
         -F 'file=@"/path/to/file.csv"' \
         -F 'property="{\"value\": \":propertyName\", \"type\": \":propertyType\", \"groupType\":  \":propertyGroupType\"}";type=application/json'
    ```
=== "HTTP"

    ```bash
    PATCH '/api/3/lookup_table/:name' HTTP/1.1
    Host: data-api.amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name=":name"; filename="file.csv"
    Content-Type: text/csv

    (data)
    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name="property"
    Content-Type: application/json

    {"value": ":propertyName", "type": ":propertyType", "groupType": ":propertyGroupType"}
    ------WebKitFormBoundary7MA4YWxkTrZu0gW--
    
    ```

### Response

=== "Success"

    ```json
    {
        "appId": "<projectId>",
        "name": "example-lookup",
        "columnHeaders": [
            "Language"
        ],
        "createdAt": 1715912516,
        "createdBy": "api",
        "lastModifiedAt": 1715912516,
        "lastModifiedBy": "api",
        "isDeleted": false,
        "isConfigured": true,
        "keyColumnHeader": "SKU",
        "keyProperty": {
            "type": "event",
            "value": "example",
            "groupType": "User"
        },
        "fileName": "lookup-table-example.csv",
        "rowCount": 3,
        "sizeBytes": 0,
    }
    ```

## Delete a Lookup Table

Delete a Lookup Table.

### Parameters

|<div class="big-column">Name</div>| Required| Type| Description|
|-----|------|----|----|
|`name` | <span class="required">Required</span>| String| Name of the table.|

### Example request

=== "cURL"

    ```curl
    curl -L -X DELETE 'https://data-api.amplitude.com/api/3/lookup_table/:name' \
         -u API_KEY:SECRET_KEY
    ```

=== "HTTP"

    ```bash
    DELETE /api/3/lookup_table/:name HTTP/1.1
    Host: data-api.amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    ```

### Response

=== "Success"

    ```json
    {
        "message": "Lookup table <:name> deleted successfully",
        "success": true
    }
    ```

## List all Lookup Tables

List all the Lookup Tables for the project.

### Example request

=== "cURL"

    ```curl
    curl -L -X GET 'https://data-api.amplitude.com/api/3/lookup_table' \
         -u API_KEY:SECRET_KEY
    ```

=== "HTTP"

    ```bash
    GET /api/3/lookup_table HTTP/1.1
    Host: data-api.amplitude.com
    Authorization: Basic {{api-key}}:{{secret:key}}
    ```

### Response

=== "Success"

    ```json
    [
        {
            "appId": "<projectId>",
            "name": "example-lookup",
            "columnHeaders": [
                "Language"
            ],
            "createdAt": 1715912516,
            "createdBy": "api",
            "lastModifiedAt": 1715912516,
            "lastModifiedBy": "api",
            "isDeleted": false,
            "isConfigured": true,
            "keyColumnHeader": "SKU",
            "keyProperty": {
                "type": "event",
                "value": "example",
                "groupType": "User"
            },
            "fileName": "lookup-table-example.csv",
            "rowCount": 3,
            "sizeBytes": 5,
        },
        {
            "appId": "<projectId>",
            "name": "example-lookup-2",
            "columnHeaders": [
                "Language"
            ],
            "createdAt": 1715912516,
            "createdBy": "api",
            "lastModifiedAt": 1715912516,
            "lastModifiedBy": "api",
            "isDeleted": false,
            "isConfigured": true,
            "keyColumnHeader": "SKU",
            "keyProperty": {
                "type": "event",
                "value": "example",
                "groupType": "User"
            },
            "fileName": "lookup-table-example.csv",
            "rowCount": 50,
            "sizeBytes": 10,
        }
    ]
    ```

## Error Codes

All the above lookup table APIs share common error codes as described below.

### Structure

|<div class="big-column">Name</div>| Description|
|-----|------|
|`statusCode` | Http status code of error. 400, 409, 413 |
|`message` | Human readable message describing the error | 
|`errorCode` | Static error code string |
|`extraParams`| Each error might have extra parameters in the response to help better point to the exact reason for the error|

### Types

|<div class="big-column">Code</div>| Description|
|-----|------|
|LOOKUP_TABLE_INVALID_FILE_COUNT| Attempted to upload more than 1 file for a single lookup table.|
|LOOKUP_TABLE_INVALID_FILE_SIZE | Created/Edited a lookup table using a file bigger than 100mb. |
|LOOKUP_TABLE_INVALID_FILE_TYPE| Created/Edited a lookup table using a file that wasn't a CSV. |
|LOOKUP_TABLE_INVALID_KEY_COLUMN| Created a lookup table with a "key" input not present in the headers of the table.|
|LOOKUP_TABLE_INVALID_VALUE_COLUMN| Cell in uploaded file exceeds 1,024 characters. |
|LOOKUP_TABLE_INVALID_KEY_PROPERTY| Provided key property doesn't exist in Amplitude. |
|LOOKUP_TABLE_INVALID_NUMBER_OF_ROWS| Created/Edited a lookup table using a file with more than 1mil rows.|
|LOOKUP_TABLE_KEY_COLUMN_DUPLICATE_VALUES| Specified key column has duplicate values.|
|LOOKUP_TABLE_INVALID_TABLE_NAME| Provided name is invalid.|
|LOOKUP_TABLE_MALFORMED_CSV| Provided CSV not processed correctly. See error message for more details.|
|LOOKUP_TABLE_INVALID_INPUT| Input for field doesn't match expectation. See error message for more details.|
|LOOKUP_TABLE_ALREADY_EXISTS| Created a table that already exists in the provided project.|
|LOOKUP_TABLE_DOES_NOT_EXIST| Attempted to load or edit table that doesn't exist.|
|LOOKUP_TABLE_INVALID_COLUMN_HEADERS| Column headers in file not processed correctly.|
