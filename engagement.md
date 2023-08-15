# CDPEngagement Class Documentation

Hey Brad! Here's a deep dive into the Salesforce Apex class `CDPEngagement`.

## Overview

The `CDPEngagement` class is designed to fetch email engagement data for a given email address from Salesforce. The data returned will contain email engagement records based on the provided email address, and will be enriched with classification and data source details.

## Methods

### 1. `getEngagement(String emailAddress): List<Object>`

- **Parameters**:
  - `emailAddress` (String) : The email address for which the engagement data is to be fetched.
  
- **Return**: A list of email engagement objects.
  
- **Description**: This method constructs a SOQL query to fetch email engagement data related to the given email address. It then sends a POST request to the `CDP_API` endpoint. On a successful response, the method will enrich each engagement object with its classification type, data source ID, and an encoded subject line.

### 2. `updateDataSourceId(Map<String, Object> engageMap): void`

- **Parameters**:
  - `engageMap` (Map<String, Object>) : The engagement map containing the data source ID.
  
- **Description**: This method checks if the given engagement map contains a data source ID, and if so, updates the data source ID based on whether it starts with 'CVENT'.

### 3. `determineDataSource(String dataSourceId): String`

- **Parameters**:
  - `dataSourceId` (String) : The data source ID to be determined.
  
- **Return**: The source of the data (`CVENT` or `Marketing Cloud`).

- **Description**: Returns 'CVENT' if the given data source ID starts with 'CVENT', otherwise returns 'Marketing Cloud'.

### 4. `determineClassification(String emailClassification, Map<String, String> classifications): String`

- **Parameters**:
  - `emailClassification` (String) : The email classification type to determine.
  - `classifications` (Map<String, String>) : A map of classifications.

- **Return**: The email classification if it exists in the given classifications map, otherwise returns 'Marketing'.

- **Description**: Determines the email classification type based on the given classifications map.

### 5. `encodeSubjectLine(Map<String, Object> engageMap): void`

- **Parameters**:
  - `engageMap` (Map<String, Object>) : The engagement map containing the subject line text.
  
- **Description**: If the given engagement map contains a subject line text, this method encodes the subject line using `unescapeHtml4()` method.

### 6. `fetchClassifications(): Map<String, String>`

- **Return**: A map of classifications based on the `CDP_Engagement_API__mdt` metadata.

- **Description**: Fetches all the custom metadata records of type `CDP_Engagement_API__mdt`, and constructs a map of classifications using the `classification__c` and `Email_Classifications__c` fields.

### Notes

- The method `getEngagement` sets a timeout of 120 seconds (or 2 minutes) for the HTTP request.
- The main logic depends on a successful response from the API (status code: 201). If the response status code is different, the function will not process the data further.
- The email engagement object can be enriched with further data, as seen with the `updateDataSourceId` and `encodeSubjectLine` methods.
