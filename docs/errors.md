# Error Handling

## Overview
This guide describes the error responses returned by the Employee Management API and explains how to interpret HTTP error status codes, identify their causes, and resolve failed requests.

## Error Response Structure
All documented error responses are returned in JSON format and contain two required string fields.  
```json
{  
    "error": "Invalid employee ID",
    "message": "Employee ID must be a positive integer."

}
```

## Error Response Field
|Response fields|Type|Required|Description|
|----|----|---|---|
|`error`|string|Yes|Brief description of the error.|
|`message`|string|Yes|Additional details about the cause of the error.|

## HTTP Error Status Codes
|Status Code| Description|
|---|----|
|400 Bad Request| The request contains missing or invalid data.|
|401 Unauthorized|The bearer token is missing or invalid.|
|404 Not Found| The requested employee does not exist.|
|409 Conflict| The request conflicts with existing data or dependencies.|
|422 Unprocessable Content| The request contains semantically invalid data.|
|500 Internal Server Error| An unexpected server error occurred.|

## Error Details

### 400 Bad Request
This status code is returned when request contains invalid or missing data.  

|Operation| Possible cause|  
|---|---|
|Retrieve/Delete employee| Invalid employee ID|
|Search employees| Invalid query parameter|
|Create employee|Missing required fields or invalid input|
|Update employee|Invalid employee ID or request body|

**Example JSON**:
```json
{
    "error": "Invalid employee ID",
    "message": "Employee ID must be a positive integer."
}
```
#### Invalid employee ID
**Resolution**: Ensure that `employeeId` is a positive integer before resubmitting the request.

#### Invalid query parameter
**Resolution**: Ensure `pageSize` is between 1 and 100. Verify other query parameter constraints.

#### Missing required fields
**Resolution**: Provide all required fields with valid values.

#### Invalid update request
**Resolution**: Provide a valid employee ID and at least one supported update field.

### 401 Unauthorized
**Possible cause**: This status code is returned when the access token is missing or invalid.  

**Example JSON**:
```json
{
    "error": "Access token error.",
    "message": "The bearer token is missing or invalid."
} 
```
**Resolution**: Ensure that the request includes a valid bearer token in the `Authorization` header.
> **Note**: Refer to the Getting Started guide for authentication instructions.

### 404 Not Found
**Possible cause**: This status code is returned when the requested employee record does not exist.

**Example JSON**:
```json
{
    "error": "The requested employee record is not found.",
    "message": "Employee ID does not exist."
}
```
**Resolution**: Verify that the employee ID identifies an existing employee record. Use the Search Employees endpoint to find a valid employee ID before resubmitting the request.

### 409 Conflict
**Possible cause**: This status code is returned when the request conflicts with existing data or dependencies.  

**Example JSON**:
```json
{
    "error": "The request conflicts with existing employee data or dependencies.",
    "message": "Employee cannot be deleted because they are assigned as a manager to other employees."
}
```
**Resolution**: Reassign the employees reporting to the specified manager or remove their manager assignments before attempting to delete the employee record again.

#### Duplicate email address
**Example JSON**:
```json
{
    "error": "The request conflicts with existing employee data or dependencies.",
    "message":  "The supplied email address already belongs to another employee."
}
```
**Resolution**: Provide an email address that is not already associated with another employee before resubmitting the request.


### 422 Unprocessable Content
**Possible cause**: This status code is returned when the request is syntactically valid but contains semantically invalid data.  
**Example JSON**:
```json
{
    "error": "The request is syntactically valid but contains semantically invalid data.",
    "message": "The supplied non-null managerId does not identify an existing employee."
}
```
**Resolution**: Ensure that `managerId` identifies an existing employee. If a manager assignment is not required, omit `managerId` when creating an employee or set it to `null` when clearing an existing manager assignment through `PATCH.`

### 500 Internal Server Error
**Possible cause**: This status code is returned when an unexpected server error occurred while processing the request.
**Example JSON**:
```json
{
    "error": "Unexpected server failure." ,
    "message": "An unexpected server error occurred while processing the request."
}
```
**Resolution**: Retry the request. Before retrying a request that creates, updates, or deletes employee data, verify whether the original operation succeeded. If the error persists, contact the API administrator.

## Endpoint-Specific Error Codes

|Status code|Retrieve|Search|Create|Update|Delete|
|---|---|---|---|---|--|
|400|Yes|Yes|Yes|Yes|Yes|
|401|Yes|Yes|Yes|Yes|Yes|
|404| Yes|No|No|Yes | Yes|
|409| No|No|Yes|Yes|Yes|
|422|No|No|Yes|Yes|No|
|500|Yes|Yes|Yes|Yes|Yes|

## Retry Guidelines
|Status Code| Recommended action|
|---|---|
|400| Correct the request before retrying.
|401|Provide a valid bearer token before retrying.|
|404|Verify the employee ID before retrying.|
|409|Resolve the conflict before retrying.|
|422|Correct the semantically invalid data before retrying.|
|500|Retry cautiously after verifying the operation outcome where applicable.|

## Related Documentation
- [Getting Started](getting-started.md)
- [API Reference](api-reference.md)
- [OpenAPI Specification](../openapi.yml)
- [README](../README.md)