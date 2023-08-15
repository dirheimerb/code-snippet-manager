# Data Cloud Authentication

`CDP_APIUTLS`

## **CDP (Customer Data Platform) Interaction**: The class is an integration utility class between Sales Cloud and Data Cloud (CDP). This integration retrieves data about a person (or contact) based on different search criteria like email, phone, or Salesforce record ID

### **Methods**: It consists mainly of three public static methods

    - `getSearchString`: Construct the search string based on search criteria.
    - `getQueryAPIResponse`: Retrieve data based on a search query.
    - `getUnifiedProfileViewResponse`: Retrieve a unified profile view based on a Salesforce record ID.

### **Constants and Metadata**

    - The class has some constants like `QUERY_API`, `CONTACTID_PREFIX`, and `LEADID_PREFIX`.
    - It retrieves data source ID from custom metadata (`CDP_Source_Name__mdt`). This could be used for filtering data from a specific source in the CDP.

### **`getSearchString` Method**

    - The method constructs an SQL-like query string for the CDP based on the search type provided (email, phone, mobile phone, or Salesforce ID).
    - The strings generated are specific to the schema of the CDP and seem to involve several join operations between tables.
    - Error handling here logs any issues and throws an AuraHandledException, which makes it suitable for Lightning Components.

### **`getQueryAPIResponse` & `getUnifiedProfileViewResponse` Methods**

    - Both these methods construct an SQL query and make an HTTP POST request to the CDP API.
    - They handle the responses and deserialize them into a Salesforce custom Apex class (`CDP_SearchModels.CDPUnifiedProfileResponse`).
    - Named Credentials (`callout:CDP_API`) are used for endpoint URLs, providing an added layer of security.
    - They've set a generous timeout of 120 seconds for the HTTP callout.
    - Exception handling is thorough, with errors being logged using a `Logger` utility class.
