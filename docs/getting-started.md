# Getting Started with the Employee Management API

## Overview
The Employee Management API enables developers to retrieve, search, create, update and delete employee records. This guide walks you through authenticating and making your first API request using Postman or cURL.

## Before you begin
Make sure you have the following:    
- An API client, such as Postman or cURL.  
- Access to the API base URL.  
- A valid bearer token provided by the API administrator.  
- Network access to the API environment.  
- The ID of an existing employee.  

## Base URL
All API requests must use the following illustrative base URL: `https://api.example.com`
Append the endpoint path to the base URL to construct the complete request URL.
> Example:   
- **Base URL**: `https://api.example.com`    
- **Endpoint path**: `/api/v2/employees/1058`   
- **Complete request URL**: `https://api.example.com/api/v2/employees/1058`

> **Note**: The base URL is illustrative and does not represent a live API server. 


## Authentication
The API uses Bearer token authentication for all API requests. If the access token is missing, expired or invalid, the API returns a `401 Unauthorized` error response.  
Include the Bearer token in the `Authorization` header as follows:

|Header|Value|Required|
|----|----|---|
|`Authorization`|`Bearer <token>`|Yes|  
Replace `<token>` with a valid access token.

> Note: Developers must not expose bearer tokens in publicly accessible repositories or client-side source code.

## Make your first API request

### Using cURL
The following instructions use Visual Studio Code and Git Bash.
Ensure that both are installed and cURL is available.

1. Open Visual Studio Code.
2. Select **Terminal > New Terminal**.
3. Select **Git Bash** as the terminal profile.
4. Copy the following cURL command:
    ```bash
    curl -sS -X GET "https://api.example.com/api/v2/employees/1058" \
    -w "\nHTTP Status: %{http_code}\n" \
    -H "Authorization: Bearer <token>" \
    -H "Accept: application/json"
    ``` 
5. Modify the command as follows:
    -  Replace the illustrative base URL with the base URL of your API environment. 
    - Replace `1058` with the ID of an existing employee.
    - Replace `<token>` with your valid access token.
6. Run the command in the terminal.

#### Expected result
The terminal displays `HTTP Status: 200`, indicating a successful request. Verify that the response body contains the requested employee details in JSON format.

### Using Postman
1. Open Postman.
2. Create a new HTTP request.
3. Select `GET` from the HTTP method dropdown.
4. Enter the request URL: `https://api.example.com/api/v2/employees/1058`
5. Modify the request URL as follows:
    -  Replace the illustrative base URL with the base URL of your API environment. 
    - Replace `1058` with the ID of an existing employee.
    
6. In the **Authorization** tab, select **Bearer Token** from the **Auth Type** dropdown.
7. Enter your valid access token in the **Token** field.
8. In the **Headers** tab, optionally configure the `Accept` header by setting the **Key** to `Accept` and the **Value** to `application/json`.
9. No request body is required for this endpoint. 
10. Click **Send** to send the request.

#### Expected result
Postman displays the HTTP status code `200 OK`, indicating a successful request. Verify that the response body contains the requested employee details in JSON format.

## Expected response
A successful request returns the HTTP status code `200 OK`. The response body contains the requested employee details in JSON format. 
Example response:
```json
{
  "employeeId": 1058,
  "firstName": "Anita",
  "lastName": "Sharma",
  "email": "anita@example.com",
  "department": "Engineering",
  "status": "active"
}
```

## Common errors
The following table describes common errors you may encounter when making your first API request and how to resolve them.

|**HTTP status Code**|**Possible cause**|**Resolution**|
|---|----|----|
|`400 Bad Request`| Employee ID is not a positive integer.| Replace the employee ID with a positive integer and resend the request.|
|`401 Unauthorized`| Access token is missing, invalid, or expired.|Provide a valid access token in the `Authorization` header and resend the request.|
|`404 Not Found`|Employee does not exist with the specified ID.|Verify the employee ID and resend the request with the ID of an existing employee.|
|`500 Internal Server Error`|An unexpected server-side error occurred.|Retry the request after a short interval. If the error persists, contact technical support.|

## Next steps
After successfully making your first API request,
explore the following resources to learn more about
the API and integrate it into your application:

- [API Overview](overview.md): Learn about the API
  capabilities and supported operations.

- [API Reference](api-reference.md): Explore the
  available endpoints, parameters, request bodies,
  responses, and error codes.

- [OpenAPI Specification](../openapi.yml): Explore
  the machine-readable API contract.