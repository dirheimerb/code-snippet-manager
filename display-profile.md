# `CDP_UnifiedProfileData` Apex Class

## **Overview:**

The `CDP_UnifiedProfileData` class is designed to interact with and retrieve a unified profile response. This class plays an integral role in searching and retrieving data from a Customer Data Platform (CDP). This Salesforce Apex class contains utility methods to sort different kinds of records such as Email Addresses, Phone Numbers, Addresses, and Party Identifications based on their last modified date.

### **Attributes:**

- `testdata`: A static string for testing purposes.
- Various string constants (`CONTACTID_PREFIX`, `LEADID_PREFIX`, etc.) which are essentially identifiers and object names that are used in Salesforce.
- `SALESCLOUD_NAME`: Retrieves data source name from a custom metadata type `CDP_Source_Name__mdt`.

## Main Method: getCDPResponse(String searchExpression, String searchType)

### **Parameters:**

- `searchExpression`: The value against which the search is to be executed. Example: `abc@test1.com`.
- `searchType`: Defines the type of the search expression; can be "Email", "phone", or "mobile phone".

### **Returns:**

- An object of type `CDP_SearchModels.UIFinalResponseWrapper` that contains the final formatted response which will be mapped back to the user interface.

### **Description:**

This method is utilized to obtain a unified profile response based on the `searchExpression` and `searchType`. Depending on whether a test is running or not, the method either deserializes the test data or obtains the response from the `CDP_APIUtls.getQueryAPIResponse()` method.

The data is then processed, parsed, and mapped to create a final UI response which contains information such as first name, last name, contact points (like email, phone), data source points, etc.

### **Highlights:**

- Uses static data for test scenarios.
- Makes use of various helper methods (which aren't provided but are inferred from the code) to process and organize data like `getDataSourceName()`, `getUPID()`, `getDataSourceRecordId()`, and `getUserAccessForRecord()`.
- The method also contains logic to display and parse different types of contact points such as email, phone, address, etc.
- There's a comment suggesting that logs were commented out, indicating a conscious choice to reduce logging verbosity.
- Exception handling includes capturing the exception message and logging it.

### Additional Methods

#### 1. getDataSourceName

- **Purpose**: Fetches data source names from Custom Metadata.
  
- **Returns**: A map where the key is the Source ID and the value is the Data Source Name.

- **Signature**:

  ```java
  private static Map<String, String> getDataSourceName()
  ```

#### 2. parseName

- **Purpose**: Parses the full name from the given `currentResponse` and returns it split by a comma.

- **Parameters**:
  - `currentResponse` - The CDP current response to be parsed.

- **Returns**: A list containing the split name (First name, Last name).

- **Signature**:

  ```java
  private static List<String> parseName(CDP_SearchModels.CDPUnifiedProfile currentResponse)
  ```

#### 3. getUserAccessForRecord

- **Purpose**: Determines the user's access for the sales cloud record. 

- **Parameters**:
  - `UPIDs` - List of unique UPIDs.
  - `dataSourceNameMap` - A map containing data source org id and data source name.
  - `unifiedProfileResponse` - All CDP responses.
  - `responseSize` - The size of the CDP response.

- **Returns**: A map with user access record id and a boolean indicating whether the user has read access.

- **Signature**:
  
  ```java
  private static Map<String, Boolean> getUserAccessForRecord(List<String> UPIDs, Map<String, String> dataSourceNameMap, CDP_SearchModels.CDPUnifiedProfileResponse unifiedProfileResponse, Integer responseSize)
  ```

#### 4. checkUsersAccessToRecord

- **Purpose**: Validates if a user has access to a specific record. It will set flags `canCreateContact` and `isSalesforceRecord` based on access.

- **Parameters**:
  - `userRecordAccessMap` - Map containing record id and a flag indicating if the user has read access.
  - `responseObj` - The response object to map back to the UI.
  - `dataSourceRecordId` - The sales cloud record id to validate access for.

- **Returns**: An object representing the data source point.

- **Signature**:
  
  ```java
  private static CDP_SearchModels.DataSourcePoint checkUsersAccessToRecord(Map<String, Boolean> userRecordAccessMap, CDP_SearchModels.UIFinalResponse responseObj, String dataSourceRecordId)
  ```

#### 5. getUPID

- **Purpose**: Extracts all unique UPIDs from the provided CDP response and stores them in a list.

- **Parameters**:
  - `responseSize` - The size of the CDP response.
  - `unifiedProfileResponse` - All CDP responses.
  - `UPIds` - List to store all unique UPIDs.

- **Returns**: List of unique UPIDs.

- **Exceptions**: Throws an `AuraHandledException` if there's an error during extraction.

- **Signature**:

  ```java
  public static List<String> getUPID(Integer responseSize, CDP_SearchModels.CDPUnifiedProfileResponse unifiedProfileResponse, List<String> UPIds)
  ```

---

## **Supporting Methods**

### **Method 1: parsePartyIDResponse**

**Purpose:**
Parse the Party ID response.

**Parameters:**

- `CDP_SearchModels.CDPUnifiedProfile currentResponse`: The current response from CDP Unified Profile.
- `CDP_SearchModels.UIFinalResponse CDPDataUIResponse`: The response to be mapped on the UI.

**Notes:**

- Extracts and sets the `identifierName` and `identificationNumber` from the `currentResponse.ContactPoint`.
- Uses `sortPartyIdentifications` method to sort party ID.
- Error handling includes logging the error and throwing an `AuraHandledException`.

---

### **Method 2: parseAddressResponse**

**Purpose:**
Parse the Address response.

**Parameters:**

- `CDP_SearchModels.CDPUnifiedProfile currentResponse`: The current response from CDP Unified Profile.
- `CDP_SearchModels.UIFinalResponse CDPDataUIResponse`: The response to be mapped on the UI.

**Notes:**

- Parses the full address into separate components like `addressLine1`, `addressLine2`, `cityName`, `postalCode`, `stateProvinceName`, `countryName`, `stateProvinceCode`, and `countryCode`.
- Uses `sortAddresses` method to sort addresses.
- Error handling includes logging the error and throwing an `AuraHandledException`.

---

### **Method 3: parsePhoneResponse**

**Purpose:**

Parse the Phone response.

**Parameters:**

- `CDP_SearchModels.CDPUnifiedProfile currentResponse`: The current response from CDP Unified Profile.
- `CDP_SearchModels.UIFinalResponse CDPDataUIResponse`: The response to be mapped on the UI.
- `String contactType`: To check if the value is for phone or mobile phone.
- `Boolean isContactType`: A flag to check the value whether it is phone or mobile phone.

**Notes:**

- Parses and sets the `telephoneNumber`, `isContactType`, and `contactType`.
- Uses `sortPhoneNumbers` method to sort phone numbers.
- Error handling includes logging the error and throwing an `AuraHandledException`.

---

### **Method 4: parseEmailResponse**

**Purpose:**

Parse the Email and Secondary Email response.

**Parameters:**

- `CDP_SearchModels.CDPUnifiedProfile currentResponse`: The current response from CDP Unified Profile.
- `CDP_SearchModels.UIFinalResponse CDPDataUIResponse`: The response to be mapped on the UI.

**Notes:**

- Parses and sets the `emailAddressName`.
- Uses `sortEmailAddresses` method to sort email addresses.
- Error handling includes logging the error and throwing an `AuraHandledException`.

---

### **Method 5: getDataSourceRecordId**

**Purpose:**
Retrieve the unique data source record id, data source object name, and data source org id.

**Parameters:**

- `List<String> UPIds`: List containing all Unique UPIDs.
- `Integer responseSize`: Size of the CDP response.
- `CDP_SearchModels.CDPUnifiedProfileResponse unifiedProfileResponse`: The full CDP response.
- `List<String> dataSourceObjectNames`: List to store data source object names.
- `List<String> dataSourceRecordIds`: List to store all data source record ids.
- `Map<String, List<String>> dataSourceRecordIdMap`: Map to associate data source name with UPIDs.
- `Map<String, List<String>> dataSourceOrgIdMap`: Map to associate data source org ids with UPIDs.

**Return:**

- `Map<String, List<String>>`: A map of UPIDs to a list of data source record ids.

**Notes:**

- Processes the data and maps the `dataSourceRecordId`, `dataSourceObjectName`, and `dataSourceOrgId` to the corresponding UPID.

### Sorting Utility Method Details

#### 1. `sortEmailAddresses(list<CDP_SearchModels.EmailAddress> emailAddresses): List<CDP_SearchModels.EmailAddress>`

**Description:**  
This method sorts a list of email addresses based on the `contactPointLastUpdatedDate` in descending order. Email addresses with a null `contactPointLastUpdatedDate` are placed at the end of the list.

**Parameters:**  

- `emailAddresses`: A list of email address records.

**Returns:**  
A sorted list of email addresses.

---

#### 2. `sortPhoneNumbers(list<CDP_SearchModels.PhoneNumber> phoneNumbers): List<CDP_SearchModels.PhoneNumber>`

**Description:**  
This method sorts a list of phone numbers based on the `contactPointLastUpdatedDate` in descending order. Phone numbers with a null `contactPointLastUpdatedDate` are placed at the end of the list.

**Parameters:**  

- `phoneNumbers`: A list of phone number records.

**Returns:**  
A sorted list of phone numbers.

---

#### 3. `sortAddresses(list<CDP_SearchModels.Address> addresses): List<CDP_SearchModels.Address>`

**Description:**  
This method sorts a list of addresses based on the `contactPointLastUpdatedDate` in descending order. Addresses with a null `contactPointLastUpdatedDate` are placed at the end of the list.

**Parameters:**  

- `addresses`: A list of address records.

**Returns:**  
A sorted list of addresses.

---

#### 4. `sortPartyIdentifications(list<CDP_SearchModels.PartyIdentification> partyIdentifications): List<CDP_SearchModels.PartyIdentification>`

**Description:**  
This method sorts a list of party identifications (specifically physician registration numbers) based on the `contactPointLastUpdatedDate` in descending order. Party identifications with a null `contactPointLastUpdatedDate` are placed at the end of the list.

**Parameters:**  

- `partyIdentifications`: A list of party identification records.

**Returns:**  
A sorted list of party identifications.

---

### Additional Notes

- **Sorting Mechanism:** The sorting mechanism used in all these methods is the Selection Sort algorithm. This algorithm divides the list into a sorted and an unsorted region and repeatedly selects the smallest (or largest, depending on the sorting order) element from the unsorted region and moves it to the beginning (or end) of the sorted region.

- **Handling Null Last Modified Dates:** For each of these sorting methods, records with a null `contactPointLastUpdatedDate` are separated out at the beginning and then added to the end of the list post sorting of non-null dated records. This ensures that the sorting order remains consistent with records containing dates sorted in descending order followed by records with null dates.

- **Commented logs:** Reduced log verbosity on January 5, 2022.
