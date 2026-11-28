# Authentication

## Overview
This guide explains how to authenticate requests to the Employee Management API using bearer tokens, resolve authentication errors, and securely manage authentication credentials.

## Authentication Method
The Employee Management API uses HTTP bearer authentication. All API requests must include a valid bearer token in the `Authorization` header.

## Before You Begin
Make sure you have the following:
- An API client, such as Postman or cURL.
- Network access to the API environment. 
- Access to the API base URL.

## Obtain a Bearer Token
To obtain a valid bearer token, contact the API administrator.

## Authenticate API Requests

### Authorization Header

Include the following header in every API request: 
```http
"Authorization": "Bearer <token>"
```


> **Note**: `https://api.example.com` is an illustrative base URL and must be replaced with the actual API base URL.

### Using cURL
Include the `Authorization` header in the cURL command to authenticate  API requests:

```bash
 curl -sS -X GET "https://api.example.com/api/v2/employees/1058" \
    -w "\nHTTP Status: %{http_code}\n" \
    -H "Authorization: Bearer <token>" \
    -H "Accept: application/json"
```
Replace `<token>` with your valid bearer token.

If authentication succeeds, the employee exists, and the request satisfies all other validation requirements, the API returns `200 OK` with the requested employee details in JSON format.

### Using Postman
1. Create a new HTTP request in Postman.
2. Select the `GET` method.
3. Enter the request URL: `https://api.example.com/api/v2/employees/1058`. Replace `1058`  with a valid employee ID.
4. Open the **Authorization** tab.
5. Select **Bearer Token** from the **Auth Type** dropdown.
6. Enter your valid bearer token in the **Token** field.
    > **Note**: Enter only the token value in the Token field, without the `Bearer` prefix.
7. In the **Headers** tab, add `Accept: application/json`.
8. Click **Send**.

Postman automatically includes the following header in the request:
```http
 Authorization: Bearer <token>
```
**Expected result**: If the request is valid and the employee exists, the API returns 200 OK with the requested employee details in JSON format.

## Protected Endpoints
All five documented API operations require bearer authentication.

 | Operation| HTTP method| Endpoint|
 |---|---|----|
 |Retrieve| `GET` |`/api/v2/employees/{employeeId}`|
 |Search | `GET`| `/api/v2/employees`|
 |Create| `POST` |`/api/v2/employees`|
 |Update | `PATCH` |`/api/v2/employees/{employeeId}`|
 |Delete | `DELETE` |`/api/v2/employees/{employeeId}` |

## Authentication Errors
If the bearer token is missing or invalid, the API returns a `401 Unauthorized` error response. 
```json
{
  "error": "Access token error.",
  "message": "The bearer token is missing or invalid."
}
```
Ensure that the request includes a valid bearer token in the `Authorization` header before resubmitting the request.

## Security Best Practices
Follow these recommendations to protect your authentication credentials and prevent unauthorized access:
- Always use HTTPS when sending API requests.
- Store bearer tokens securely using environment variables or secret management tools.
- Do not hardcode bearer tokens in application source code.
- Never commit authentication credentials to version control repositories.
- Include bearer tokens in the `Authorization` header rather than in request URLs.
- Avoid exposing bearer tokens in application logs, screenshots, or shared documentation.
- If a bearer token is compromised, contact the API administrator to request its revocation and replacement.


## Related Documentation
- [Getting Started](getting-started.md)
- [API Reference](api-reference.md)
- [Error Handling](errors.md)
- [OpenAPI Specification](../openapi.yml)
- [README](../README.md)