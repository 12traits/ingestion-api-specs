# Errors

12traits API uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the `2xx` range indicate success. Codes in the `4xx` range indicate an error that failed given the information provided. Codes in the `5xx` range indicate an error with 12traits servers (these are rare).

The 12traits Ingestion API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request - Please check the Reference
401 | Unauthorized - API Key is not valid, or game ID is not valid
404 | Not Found - The specified resource not found
500 | Internal Server Error - We had a problem with our server. Try again later.

## Error Response Format

```shell
{
    "code": 400,
    "message": "bad request",
    "errors": {
        "email": "must be a valid email address"
    }
}
```