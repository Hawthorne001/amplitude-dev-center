---
title: Lookup Table API
description: The Lookup Table API lets you create, get, update, and delete lookup tables to augment your data.
---

!!!deprecated "This version of the Lookup Tables API is deprecated"
    This API is no longer supported. Use the new [API](/data/apis/lookup-tables-api). 

!!!beta "This feature is in beta"
    This requires that you have the `lookup_table` feature enabled in Amplitude. Contact your Amplitude account manager if you need help.

Lookup tables let you augment user and event properties. Instead of using formulas, you can upload a CSV file that contains property mappings to derive new properties. 

To create a lookup property, create a lookup table to reference. You can retrieve and update each of the tables using the API. Lookup Tables are identified by the name and are scoped per project.

You can also create and manage lookup tables from the Amplitude web app. See [Lookup Table](/data/sources/lookup-table) for more information.

--8<-- "includes/postman.md"

## Endpoints

| Region | Endpoint |
| --- | --- |
| Standard Server | https://amplitude.com/api/2/lookup_table |
| EU Residency Server | https://analytics.eu.amplitude.com/api/2/lookup_table |

--8<-- "includes/auth-basic.md"

## Considerations

The CSV file must comply with the following requirements:

- The max file size is 100 MB and the file can't have more than 1,000,000 rows.
- The first row must contain column names/headers.
- The first column must correspond to the mapping property value and must contain *unique* values. Lookup Tables search for exact matches, and are *case-sensitive*.
- Columns must be separated by commas.
- Rows must be separated by line breaks.
- If a field value contains commas or quotes, it should be wrapped within double quotation marks. The first double quote signifies the beginning of the column data, and the last double quote marks the end. If the value contains a string with double quotes, these are replaced by two double quotes `""`.

## Create a Lookup Table

Create a Lookup Table object by uploading a CSV that maps an existing property to the new properties to create. Send the request with the type multipart/form-data type.

### Parameters

|<div class="big-column">Name</div>| Description|
|-----|------|
|`name` | <span class="required">Required</span>. String. Name of the table.|
|`file` | <span class="required">Required</span>. File. A CSV representation of the mappings.|

### Example request

=== "cURL"

    ```curl
    curl -L -X POST 'https://amplitude.com/api/2/lookup_table/:name' \
         -u API_KEY:SECRET_KEY \
         -F 'file=@"/path/to/file.csv"' \
    ```

=== "HTTP"

    ```bash
    POST '/api/2/lookup_table/:name' HTTP/1.1
    Host: api2.amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name=":name"; filename="file.csv"
    Content-Type: text/csv

    (data)
    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    ```

### Responses

=== "Success"

    ```json
    {
        "name": "skuToMetadata",
        "column_headers": [
            "Product Category",
            "Product Name"
        ],
        "created_at": "2021-07-15T21:04:23.000593",
        "created_by": "rest",
        "last_modified_at": "2021-07-16T19:14:11.627477",
        "last_modified_by": "rest"
    }
    ```
=== "HTTP 400: Bad Request"

    ```bash
    HTTP 400: Bad Request
    ```

    - Invalid file
    - File type is invalid. Accepted file types are `text/csv`, `text/plain`, and `text/tab-separated-values`.
    - File is empty
    - Found duplicate column header. There's a duplicate column, please remove the column so the file can be processed.

=== "HTTP 409: Conflict"

    ```bash

    HTTP 409: Conflict (Conflict, name already exists)
    ```

    The table already exists

=== "HTTP 413: Payload Too Large"

    ```bash

    HTTP 413: Payload Too Large
    ```

    The file exceeds the max size.

## Retrieve a Lookup Table

Retrieve a Lookup Table by its name.

### Parameters

|<div class="big-column">Name</div>| Description|
|-----|------|
|`name` | <span class="required">Required</span>. String. Name of the table.|

### Example request

=== "cURL"

    ```curl
    curl -L -X GET 'https://amplitude.com/api/2/lookup_table/:name' \
         -u API_KEY:SECRET_KEY
    ```

=== "HTTP"

    ```bash
    GET /api/2/lookup_table/:name HTTP/1.1
    Host: amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    ```

### Response

=== "Success"

    ```json
    {
        "name": "skuToMetadata",
        "column_headers": [
            "Product Category",
            "Product Name"
        ],
        "created_at": "2021-07-15T21:04:23.000593",
        "created_by": "rest",
        "last_modified_at": "2021-07-16T19:14:11.627477",
        "last_modified_by": "rest"
    }
    ```

=== "HTTP 404: Not found"

    ```bash
    HTTP 404: Not found
    ```

    The table wasn't found because it wasn't created

## Update a Lookup Table

Update a Lookup Table's columns and data.

### Parameters

|<div class="big-column">Name</div>| Description|
|-----|------|
|`name` | <span class="required">Required</span>. String. Name of the table.|
|`file` | <span class="required">Required</span>. File. A CSV representation of the mappings.|

### Example request

=== "cURL"

    ```curl
    curl -L -X PATCH 'https://amplitude.com/api/2/lookup_table/:name' \
         -u API_KEY:SECRET_KEY
         -F 'file=@"/path/to/file.csv"' \
    ```
=== "HTTP"

    ```bash
    PATCH /api/2/lookup_table/:name HTTP/1.1
    Host: amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    Content-Disposition: form-data; name=":name"; filename="file.csv"
    Content-Type: text/csv

    (data)
    ----WebKitFormBoundary7MA4YWxkTrZu0gW
    ```

### Response

=== "Success"

    ```json
    {
        "name": "skuToMetadata",
        "column_headers": [
            "Product Category",
            "Product Name"
        ],
        "created_at": "2021-07-15T21:04:23.000593",
        "created_by": "rest",
        "last_modified_at": "2021-07-16T19:14:11.627477",
        "last_modified_by": "rest"
    }
    ```

=== "HTTP 400: Bad Request"

    ```bash
    HTTP 400: Bad Request
    ```

    - Requires at least one modification. There should be a file attached.
    - File type is invalid. Accepted file types are `text/csv`, `text/plain`, and `text/tab-separated-values`.
    - File is empty.
    - Found duplicate column header. There's a duplicate column, please remove the column so the file can be processed.

=== "HTTP 404: Not found"

    ```bash
    HTTP 404: Not found
    ```

    The table wasn't found because it wasn't created

=== "HTTP 413: Payload Too Large"

    ```bash

    HTTP 413: Payload Too Large
    ```

    The file exceeds the max size.

## Delete a Lookup Table

Delete a Lookup Table.

### Parameters

|<div class="big-column">Name</div>| Description|
|-----|------|
|`name` | <span class="required">Required</span>. String. Name of the table.|
|`force` | <span class="optional">Optional</span>. Boolean. Delete the associated properties. Defaults to `false`.|

### Example request

=== "cURL"

    ```curl
    curl -L -X DELETE 'https://amplitude.com/api/2/lookup_table/:name' \
         -u API_KEY:SECRET_KEY
    ```

=== "HTTP"

    ```bash
    DELETE /api/2/lookup_table/:lookup_table_name?force=True HTTP/1.1
    Host: amplitude.com
    Authorization: Basic {{api-key}}:{{secret-key}}
    ```

### Response

=== "Success"

    ```json
    {
        "name": "skuToMetadata",
        "success": true
    }
    ```
=== "HTTP 404: Not found"

    ```bash

    HTTP 404: Not found
    ```

    The table wasn't found.

## List all Lookup Tables

List all the Lookup Tables for the project.

### Example request

=== "cURL"

    ```curl
    curl -L -X GET 'https://amplitude.com/api/2/lookup_table' \
         -u API_KEY:SECRET_KEY
    ```

=== "HTTP"

    ```bash
    GET /api/2/lookup_table HTTP/1.1
    Host: amplitude.com
    Authorization: Basic {{api-key}}:{{secret:key}}
    ```

### Response

```json
{
    "data": [
        {
            "name": "isbnToMetadata",
            "column_headers": [
                "Genres",
                "Authors"
            ],
            "created_at": "2021-07-15T21:04:23.000593",
            "created_by": "rest",
            "last_modified_at": "2021-07-16T19:14:11.627477",
            "last_modified_by": "rest"
        },
        {
            "name": "skuToMetadata",
            "column_headers": [
                "Product Category",
                "Product Name"
            ],
            "created_at": "2021-07-16T19:28:18.070073",
            "created_by": "rest",
            "last_modified_at": "2021-07-16T19:28:18.070073",
            "last_modified_by": "rest"
        }
    ]
}
```
