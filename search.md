# **CDP_SearchModels Class Documentation**

`CDP_SearchModels` class.

---

## **Overview**

`CDP_SearchModels` is an Apex class designed for Salesforce. It comprises a collection of nested classes that model different types of responses from the Customer Data Platform (CDP). These classes provide structures to map data from the CDP into Salesforce UI components.

---

## **Class Breakdown**

### **1. CDPUnifiedProfileResponse**

Represents a unified profile response from the CDP.

- `data`: A list containing instances of the `CDPUnifiedProfile`.

### **2. CDPUnifiedProfile**

Models the key attributes of a unified profile from the CDP.

- `Id`: The identifier for the profile.
- `ContactPoint`: The point of contact associated with the profile.
- `ContactPointType`: Type of contact point (e.g., email, phone).
- `SourceRecord`: The source record from which the profile was derived.
- `LastModifiedDate`: The last modified date of the profile.

### **3. Metadata**

Holds meta information regarding various contact points.

- `ContactPoint`: A list of different types of contact points.
- Other properties: `Id`, `OccuranceCnt`, `ContactPointType`.

### **4. ContactPoint**

Details about a specific type of contact.

- `type`: The type of the contact point.

### **5. UIFinalResponseWrapper**

A wrapper class for mapping CDP responses to the Salesforce UI.

- Contains various properties like `unifiedProfileData`, `exceptionMessage`, `firstName`, `lastName`, and more. This class provides a holistic view of the response with a mix of basic user details and contact information.

### **6. PhoneNumber**

Representation of a phone number from the CDP response.

- Includes properties like `contactType`, `isContactType`, `telephoneNumber`, and `contactPointLastUpdatedDate`.

### **7. Address**

Models the address data from the CDP response.

- Contains address components such as `addressLine1`, `addressLine2`, `stateProvinceName`, `cityName`, `postalCode`, and more.

### **8. EmailAddress**

Represents email address details.

- `emailAddressName`: The actual email address.
- `contactPointLastUpdatedDate`: Last updated timestamp for the email address.

#### **9. PartyIdentification**

Holds identification details for parties.

- `identifierName`: The name of the identifier.
- `identificationNumber`: Specific number or code for identification.
- `contactPointLastUpdatedDate`: Timestamp for the last update.

#### **10. DataSourcePoint**

Represents points from different data sources.

- `dataSourceName`: Name of the data source.
- `dataSourcerecordId`: Identifier for the record from the data source.
- `isSalesforceRecord`: A flag to determine if the data originates from a Salesforce record.

---

### **Notes**

1. The `@AuraEnabled` annotation makes the properties accessible in Salesforce's Lightning component framework.
2. This class is primarily designed for data mapping, converting raw responses from the CDP into a structured format suitable for Salesforce LWC.
3. Certain properties contain comments indicating specific ticket numbers (e.g., `SFSC-8442`).

---

## Apex

`CDP_SearchModels`

```java
public class CDP_SearchModels {
    //This classes is for to map CDP response - starts
    public class CDPUnifiedProfileResponse {
        @AuraEnabled
        public list<CDPUnifiedProfile> data;
    }
    public class CDPUnifiedProfile {
        @AuraEnabled
        public string Id { get; set; }
        @AuraEnabled
        public string ContactPoint { get; set; }
        @AuraEnabled
        public string ContactPointType { get; set; }
        @AuraEnabled
        public string SourceRecord { get; set; }
        @AuraEnabled
        public string LastModifiedDate { get; set; }
    }
    public class metadata {
        @AuraEnabled
        public list<ContactPoint> ContactPoint;
        @AuraEnabled
        public list<ContactPoint> Id;
        @AuraEnabled
        public list<ContactPoint> OccuranceCnt;
        @AuraEnabled
        public list<ContactPoint> ContactPointType;
    }
    public class ContactPoint {
        @AuraEnabled
        public string type { get; set; }
    }
    //This classes is for to map CDP response - ends
    //This classes is for to map CDP response to UI - starts
    public class UIFinalResponseWrapper {
        @AuraEnabled
        public List<UIFinalResponse> unifiedProfileData { get; set; }
        @AuraEnabled
        public string exceptionMessage { get; set; }
    }

    public class UIFinalResponse {
        @AuraEnabled
        public string firstName { get; set; }
        @AuraEnabled
        public string lastName { get; set; }
        @AuraEnabled
        public Boolean isActive { get; set; } //SFSC-8442 Added Active/Inactive Flag to 360 Search Results
        @AuraEnabled
        public string UPId { get; set; }
        @AuraEnabled
        public string dataSources { get; set; }
        @AuraEnabled
        public List<EmailAddress> contactPointEmails { get; set; }
        @AuraEnabled
        public List<PhoneNumber> phoneNumbers { get; set; }
        @AuraEnabled
        public List<Address> fullAddress { get; set; }
        @AuraEnabled
        public List<PartyIdentification> partyIdentifications { get; set; }
        @AuraEnabled
        public List<DataSourcePoint> dataSourcePoints { get; set; }
        @AuraEnabled
        public List<string> datasourceObjectNames { get; set; }
        @AuraEnabled
        public boolean canCreateContact { get; set; }
        @AuraEnabled
        public boolean isCDPRecord { get; set; }
    }
    //This classes is for to map CDP response to UI - ends
    //This classes is for to map phone number response to UI
    public class PhoneNumber {
        @AuraEnabled
        public string contactType { get; set; } // mobile, home
        @AuraEnabled
        public Boolean isContactType { get; set; }
        @AuraEnabled
        public string telephoneNumber { get; set; }
        @AuraEnabled
        public DateTime contactPointLastUpdatedDate { get; set; }
    }
    //This classes is for to map address response to UI
    public class Address {
        @AuraEnabled
        public string addressLine1 { get; set; }
        @AuraEnabled
        public string addressLine2 { get; set; }
        @AuraEnabled
        public string stateProvinceName { get; set; }
        @AuraEnabled
        public string stateProvinceCode { get; set; } //SFSC - 9860- for state code mapping
        @AuraEnabled
        public string cityName { get; set; }
        @AuraEnabled
        public string postalCode { get; set; }
        @AuraEnabled
        public string countryName { get; set; }
        @AuraEnabled
        public string countryCode { get; set; }  //SFSC - 9860- for country code mapping
        @AuraEnabled
        public DateTime contactPointLastUpdatedDate { get; set; }
    }
    //This classes is for to map email address response to UI
    public class EmailAddress {
        @AuraEnabled
        public string emailAddressName { get; set; }
        @AuraEnabled
        public DateTime contactPointLastUpdatedDate { get; set; }
    }
    //This classes is for to map party id response to UI
    public class PartyIdentification {
        @AuraEnabled
        public string identifierName { get; set; }
        @AuraEnabled
        public string identificationNumber { get; set; }
        @AuraEnabled
        public DateTime contactPointLastUpdatedDate { get; set; }
    }
    //This classes is for to map data source point response to UI
    public class DataSourcePoint {
        @AuraEnabled
        public string dataSourceName { get; set; }
        @AuraEnabled
        public string dataSourcerecordId { get; set; }
        @AuraEnabled
        public Boolean isSalesforceRecord { get; set; }
    }
}
```

## LWC Component - CDP_Search

```js
import { LightningElement, track, api, wire } from 'lwc';
import getUnifiedProfileData from '@salesforce/apex/CDP_UnifiedProfileData.getCDPResponse';
import { NavigationMixin } from 'lightning/navigation';
import { encodeDefaultFieldValues } from 'lightning/pageReferenceUtils';
import CONTACT_OBJECT from '@salesforce/schema/Contact';
import LEAD_OBJECT from '@salesforce/schema/Lead';
import { getObjectInfo } from 'lightning/uiObjectInfoApi';
//SFSC-8029: to import the custom labels to show the error messages
import mobileAdditionalMessage from '@salesforce/label/c.CDP_Search_Mobile_Additional_Error_Message';
import phoneAdditionalMessage from '@salesforce/label/c.CDP_Search_Phone_Additional_Error_Message';

const EMAIL_REGEX =
    '^[a-zA-Z0-9._|\\\\%#~`=?&/$^*!}{+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,4}[\\.\\w]*$';

export default class CdpSearch extends NavigationMixin(LightningElement) {
    /**
     * @Method {searchOptions} Handle search with dropdown
     * @param(property)
     * CdpSearch.searchOptions:
     * @param Searchvalue:
     * @type String:label
     *
     */
    searchOptions = [
        {
            value: 'email',
            label: 'Email'
        },
        {
            value: 'phone',
            label: 'Phone'
        },
        {
            value: 'mobilePhone',
            label: 'Mobile Phone'
        }
    ];

    searchKey; //input value
    isSearchButton = true; //to disable search button
    isSearchTypeDisabled = false; // to disable search combobox
    isSearchInputDisabled = true; // to disable search input
    unifiedProfiles = [];
    changedData = [];
    displayUnifiedProfileTable = false;
    @track unifiedProfileCurrentPage = [];
    redirectURL = '';
    additionalMessage = ''; //SFSC-8029 variable declaration
    startingRecord = 1; //start record position per page
    endingRecord = 0; //end record position per page
    pageSize = 5; //default value we are assigning
    totalRecountCount = 0; //total record count received from all retrieved records
    totalPage = 0; //total number of page is needed to display all records
    page = 1;
    showSpinner = false;
    isResult = false;
    isTimeOut = false;
    isFirstPage = true;
    isLastPage = true;
    showRecordTypeModal = false;
    showContactCreateModal = false;
    hasMultipleRecordTypeAccess = true;
    //SFSC-4996 Variable declaration starts
    showObjectSelectionModal = false;
    selectedObject;
    globalprofileindex;
    isCreateButtonAvailableToUser = true;
    contactCreateAccess = false;
    leadCreateAccess = false;
    //SFSC-4996 Variable declaration ends

    // model for holding contact creation data
    @api contactData = {
        firstName: null,
        lastName: null,
        globalPartyId: null,
        partyIdentificationNumber: null,
        phone: null,
        mobilePhone: null,
        extension: null,
        email: null,
        mailingStreet: null,
        otherStreet: null,
        mailingCity: null,
        mailingPostalCode: null,
        mailingCountry: null,
        mailingState: null
    };
    @api isSearchFromButtonOverride = false; //4977 changes
    CDPSearch = 'CDP'; //4977 changes
    value = '';

    handleChange(event) {
        this.value = event.detail.value;
        this.searchKey = ''; //Added if phone,email and mobile phone are selected then searchkey will be blank
        this.isSearchButton = true;
        this.isSearchInputDisabled = false;
        this.unifiedProfiles = [];
        this.displayUnifiedProfileTable = false;
        //Updated on 24 Feb 2022
        let input = this.template.querySelector('lightning-input');
        input.value = null;
        input.setCustomValidity('');
        input.reportValidity();
    }
    handleSearchExpressionChange(event) {
        this.searchKey = event.target.value;
        this.isResult = false;
        this.isTimeOut = false;
        if (this.searchKey.length === 0) {
            this.unifiedProfiles = [];
            this.displayUnifiedProfileTable = false;
            this.isResult = false;
            this.isTimeOut = false;
        } else if (this.searchKey.length > 0 && this.value === '') {
            this.isSearchButton = true;
        } else {
            this.isSearchButton = false;
        }
        // Added on 3 Feb 2022 - Reset input message
        let inputCmp = event.target;
        inputCmp.setCustomValidity('');
        let searchTerm = event.target.value;
        if (
            !searchTerm ||
            !searchTerm.length ||
            (searchTerm.length > 0 && !searchTerm.trim().length)
        ) {
            inputCmp.setCustomValidity('Complete this field.');
            this.isSearchButton = true;
        } else if (this.value === 'email' && !searchTerm.match(EMAIL_REGEX)) {
            console.log('inside sec');
            inputCmp.setCustomValidity('Please enter valid email address.');
            this.isSearchButton = true;
            // SFSC-7856 removing all characters to send the number for search update 2022-02-23
        } else if (
            // SFSC-7856 MSG update 2022-02-23
            (this.value === 'mobilePhone' || this.value === 'phone') &&
            !searchTerm.match(
                /^\+?\d{0,3}\s?[-()+.\s]?\(?\d{3}\)?[-()+.\s]?\d{3}[-()+.\s]?\d{4}?/
            ) &&
            searchTerm.replace(/\D/g, '').length < 6
        ) {
            inputCmp.setCustomValidity(
                `Please enter valid ${
                    this.value === 'phone' ? 'phone' : 'mobile'
                } number. Please enter more than 5 numbers.`
            );
            this.isSearchButton = true;
        }
        inputCmp.reportValidity();
    }

    @wire(getObjectInfo, { objectApiName: CONTACT_OBJECT })
    contactObjectInfo;

    @wire(getObjectInfo, { objectApiName: LEAD_OBJECT })
    leadObjectInfo; //SFSC-4996 To check for lead create access

    renderedCallback() {
        //SFSC - 7070 - changed from "connectedCallBack" to "rendered callback"
        //SFSC - 7070 - - added to stop calling further method again and again starts
        if (this.isRendered) return;

        window.setTimeout(
            (self) => {
                // Changed on 2 Feb 2022 - SFSC - 7070
                self.checkForMultipleRecordTypeAccess();
            },
            2500,
            this
        );
    }

    resetAll() {
        this.unifiedProfiles = [];
        this.unifiedProfileCurrentPage = [];
        this.startingRecord = 1;
        this.endingRecord = 0;
        this.pageSize = 5;
        this.totalRecountCount = 0;
        this.totalPage = 0;
        this.page = 1;
        this.displayUnifiedProfileTable = false;
    }

    // Added on 3 Feb 2022 - check input validation
    checkInputValidation() {
        let allValid = true;
        let inputCmp = this.template.querySelector('.searchInput');
        if (inputCmp) {
            let searchTerm = inputCmp.value;
            if (
                !searchTerm ||
                !searchTerm.length ||
                (searchTerm.length > 0 && !searchTerm.trim().length)
            ) {
                inputCmp.setCustomValidity('Complete this field.');
                allValid = false;
            } else if (
                this.value === 'email' &&
                !searchTerm.match(EMAIL_REGEX)
            ) {
                inputCmp.setCustomValidity('Please enter valid email address.');
                allValid = false;
            } else if (
                (this.value === 'mobilePhone' || this.value === 'phone') &&
                !searchTerm.match(
                    /^\+?\d{0,3}\s?[-()+.\s]?\(?\d{3}\)?[-()+.\s]?\d{3}[-()+.\s]?\d{4}?/
                ) &&
                searchTerm.replace(/\D/g, '').length < 6
            ) {
                inputCmp.setCustomValidity(
                    `Please enter valid ${
                        this.value === 'phone' ? 'phone' : 'mobile'
                    } number.Please enter more than 5 numbers`
                );
                allValid = false;
            }
            if (this.value === '') {
                allValid = false;
            }
            if (!allValid) {
                inputCmp.reportValidity();
            }
        }

        if (allValid) {
            this.handleSearch();
        }
    }

    handleSearch() {
        // show spinner
        this.showSpinner = true;
        //reset no result flag
        this.isResult = false;
        this.isTimeOut = false;
        this.unifiedProfileCurrentPage = [];
        this.displayUnifiedProfileTable = false;
        let _searchValue = '';
        this.resetAll();
        if (this.value === 'phone' || this.value === 'mobilePhone') {
            _searchValue = this.searchKey.replace(/\D/g, '');
        } else {
            _searchValue = this.searchKey.trim(' ');
        }
        // to disable search combo and search input
        this.isSearchTypeDisabled = true;
        this.isSearchInputDisabled = true;
        this.isSearchButton = true;
        getUnifiedProfileData({
            searchExpression: _searchValue,
            searchType: this.value
        })
            .then((result) => {
                console.log(
                    'UNIFIED_PROFILE_RECORDS',
                    result.unifiedProfileData
                );
                var unifiedProfileDataObj = result.unifiedProfileData;
                var unifiedProfileData = JSON.parse(
                    JSON.stringify(unifiedProfileDataObj)
                );
                var exceptionMessage = result.exceptionMessage;
                //Code for Pagination
                if (
                    unifiedProfileData != null &&
                    unifiedProfileData.length > 0
                ) {
                    this.totalRecountCount = unifiedProfileData.length;
                }
                this.totalPage = Math.ceil(
                    this.totalRecountCount / this.pageSize
                );
                if (
                    unifiedProfileData != null &&
                    unifiedProfileData.length > 0 &&
                    this.searchKey.length !== 0
                ) {
                    this.unifiedProfiles = unifiedProfileData;
                    this.displayUnifiedProfileTable = true;
                    this.isResult = false;
                } else {
                    this.unifiedProfiles = [];
                    this.displayUnifiedProfileTable = false;
                    this.isResult = true;
                    /*START: SFSC-8029: Added on 05-12-2022 : 
                        Added additional message in case of no search results for mobile or phone*/
                    this.addAdditionalMessage();
                    /*END: SFSC-8029: Added on 05-12-2022 : 
                        Added additional message in case of no search results for mobile or phone*/
                }
                if (exceptionMessage === 'Read timed out') {
                    this.isTimeOut = true;
                    this.isResult = false;
                }

                //append global profile data value
                this.unifiedProfileDataValue(this.unifiedProfiles);

                // logic for Pagination
                this.displayRecordPerPage(this.page);

                this.showSpinner = false;
                //To enable search combo and input
                this.isSearchTypeDisabled = false;
                this.isSearchInputDisabled = false;
                this.isSearchButton = false;

                //Start: SFSC-4996 Check if Create New button should be visible to user or not
                if (this.contactObjectInfo.data !== undefined) {
                    this.contactCreateAccess = JSON.parse(
                        JSON.stringify(this.contactObjectInfo)
                    ).data.createable;
                }
                if (this.leadObjectInfo.data !== undefined) {
                    this.leadCreateAccess = JSON.parse(
                        JSON.stringify(this.leadObjectInfo)
                    ).data.createable;
                }
                if (!this.contactCreateAccess && !this.leadCreateAccess) {
                    this.isCreateButtonAvailableToUser = false;
                }
                //End: SFSC-4996
            })
            .catch((error) => {
                // reset contacts var with null
                this.unifiedProfiles = null;
                this.showSpinner = false;
                this.isResult = true;
                /*START: SFSC-8029: Added on 05-12-2022 : 
                        Added additional message in case of no search results for mobile or phone*/
                this.addAdditionalMessage();

                /*END: SFSC-8029: Added on 05-12-2022 : 
                        Added additional message in case of no search results for mobile or phone*/
                this.displayUnifiedProfileTable = false;
                console.error('ERROR', error);
                //To enable search combo and input
                this.isSearchTypeDisabled = false;
                this.isSearchInputDisabled = false;
                this.isSearchButton = false;
                //added if exception received, then also user should be able to create contact
                this.checkObjectAccess();
            });
    }

    //SFSC-8029: Method to assign value to additional message according to search value
    addAdditionalMessage() {
        switch (this.value) {
            case 'phone':
                this.additionalMessage = phoneAdditionalMessage;
                break;
            case 'mobilePhone':
                this.additionalMessage = mobileAdditionalMessage;
                break;
            default:
                this.additionalMessage = '';
        }
    }

    unifiedProfileDataValue() {
        var i, currGlobalProfile, addresses, j, currAddress;
        for (i = 0; i < this.unifiedProfiles.length; i++) {
            currGlobalProfile = this.unifiedProfiles[i];
            addresses = currGlobalProfile.fullAddress;
            for (j = 0; j < addresses?.length; j++) {
                currAddress = addresses[j];
                currAddress.concatenatedAddress = this.getFullAddress(
                    currAddress.addressLine1,
                    currAddress.addressLine2,
                    currAddress.cityName,
                    currAddress.stateProvinceCode,
                    currAddress.stateProvinceName,
                    currAddress.postalCode,
                    currAddress.countryName
                );
            }
        }
    }

    getFullAddress(
        addressLine1,
        addressLine2,
        cityName,
        stateProvinceCode,
        stateProvinceName,
        postalCode,
        countryName
    ) {
        var fullAddress = '';
        // Added on 17 Feb 2022
        fullAddress =
            (addressLine1 != null ? addressLine1 + '. ' : '') +
            (addressLine2 != null ? addressLine2 + '. ' : '') +
            (cityName != null ? cityName + ', ' : ' ') +
            (stateProvinceName != null ? stateProvinceName + ' ' : '') +
            (postalCode != null ? postalCode + ' ' : '') +
            (countryName != null ? countryName : ' ');
        return fullAddress;
    }
    splitAndNewLine(val, seperator) {
        if (val) return val.split(seperator);
        return '';
    }
    //clicking on previous button this method will be called
    previousHandler() {
        if (this.page > 1) {
            this.page = this.page - 1; //decrease page by 1
            this.displayRecordPerPage(this.page);
        }
    }

    //clicking on next button this method will be called
    nextHandler() {
        if (this.page < this.totalPage && this.page !== this.totalPage) {
            this.page = this.page + 1; //increase page by 1
            this.displayRecordPerPage(this.page);
        }
    }

    displayRecordPerPage(page) {
        this.startingRecord = (page - 1) * this.pageSize;
        this.endingRecord = this.pageSize * page;

        this.endingRecord =
            this.endingRecord > this.totalRecountCount
                ? this.totalRecountCount
                : this.endingRecord;

        this.unifiedProfileCurrentPage = this.unifiedProfiles.slice(
            this.startingRecord,
            this.endingRecord
        );

        //logic to disable and enable previous and next button
        this.prevNextButtonHandling(page);
    }
    navigateToRecord(event) {
        var recordid = event.target.dataset.recordid;
        var datasourceindex = event.target.dataset.datasourceindex;
        var datasourceObjectNames =
            this.unifiedProfiles[datasourceindex].datasourceObjectNames;

        if (
            datasourceObjectNames.length > 0 &&
            datasourceObjectNames.includes('Contact')
        ) {
            //for(var i=0;i<dataSourceRecordIds.length ;i++){
            this[NavigationMixin.GenerateUrl]({
                type: 'standard__recordPage',
                attributes: {
                    objectApiName: 'Contact',
                    recordId: recordid,
                    actionName: 'view'
                }
            }).then((url) => {
                window.open(url);
            });
            // }
        } else if (
            datasourceObjectNames.length > 0 &&
            datasourceObjectNames.includes('Lead')
        ) {
            this[NavigationMixin.GenerateUrl]({
                type: 'standard__recordPage',
                attributes: {
                    objectApiName: 'Lead',
                    recordId: recordid,
                    actionName: 'view'
                }
            }).then((url) => {
                window.open(url);
            });
        }
    }
    //Method to disable and enable previous and next button
    prevNextButtonHandling() {
        this.isFirstPage = this.page === 1;
        this.isLastPage = this.page === this.totalPage || this.totalPage === 0;
    }
    navigateContactCreation() {
        if (this.unifiedprofileindex !== undefined) {
            this.mapSelectedUnifiedProfileRecordData();
        }
        //after clicking adopt new button of global search
        if (this.hasMultipleRecordTypeAccess) {
            this.showContactCreateModal = false;
            this.showRecordTypeModal = true;
        } else {
            this.showRecordTypeModal = false;
            this.showContactCreateModal = true;
        }
    }

    //Created this method to make the code generic for lead and contact for autofilling the record values (SFSC-4996)
    mapSelectedUnifiedProfileRecordData() {
        var phoneNumbersData,
            fullAddress,
            emailAddress,
            partyIdentifications,
            i,
            currAddress,
            latestHomePhoneNumber = '',
            latestMobilePhoneNumber = '',
            latestExtensionNumber = '',
            currentMailingStreet,
            currentMailingStateName,
            currentMailingStateCode, //SFSC - 9860 - Variable declaration
            currentOtherStreet,
            currentMailingCity,
            currentPostalCode,
            currentMailingCountry,
            currentMailingCountryCode, //SFSC - 9860 - Variable declaration
            emailAddressName,
            currEmailAddress,
            partyIdentificationNumber,
            unifiedprofileindex,
            currPartyIdentification;
        unifiedprofileindex = this.unifiedprofileindex;
        unifiedprofileindex =
            parseInt(this.pageSize * (this.page - 1), 10) +
            parseInt(unifiedprofileindex, 10); // // 11

        phoneNumbersData =
            this.unifiedProfiles[unifiedprofileindex].phoneNumbers;
        fullAddress = this.unifiedProfiles[unifiedprofileindex].fullAddress;
        emailAddress =
            this.unifiedProfiles[unifiedprofileindex].contactPointEmails;
        partyIdentifications =
            this.unifiedProfiles[unifiedprofileindex].partyIdentifications;

        //Autofill latest phone numbers
        for (i = 0; i < phoneNumbersData.length; i++) {
            if (phoneNumbersData[i].contactType.toLowerCase() === 'home') {
                latestHomePhoneNumber =
                    phoneNumbersData[i].telephoneNumber.trim(); //SFSC - 6953 - trim the space
                latestExtensionNumber = phoneNumbersData[i].extensionNumber;
                break;
            }
        }

        for (i = 0; i < phoneNumbersData.length; i++) {
            if (phoneNumbersData[i].contactType.toLowerCase() === 'mobile') {
                latestMobilePhoneNumber =
                    phoneNumbersData[i].telephoneNumber.trim(); //SFSC - 6953 - trim the space
                break;
            }
        }

        //Autofill latest Address
        for (i = 0; i < fullAddress.length; i++) {
            currAddress = fullAddress[i];
            currentMailingStreet = currAddress.addressLine1;
            currentOtherStreet = currAddress.addressLine2;
            currentMailingCity = currAddress.cityName;
            currentPostalCode = currAddress.postalCode;
            currentMailingStateName = currAddress.stateProvinceName;
            currentMailingCountry = currAddress.countryName;
            //SFSC - 9860 - Mapped state and country code - starts
            currentMailingCountryCode = currAddress.countryCode;
            currentMailingStateCode = currAddress.stateProvinceCode;
            //SFSC - 9860 - Mapped state and country code - ends
            break;
        }

        //Autofill latest email address
        for (i = 0; i < emailAddress.length; i++) {
            currEmailAddress = emailAddress[i];
            emailAddressName = currEmailAddress.emailAddressName;
            break;
        }
        for (i = 0; i < partyIdentifications.length; i++) {
            currPartyIdentification = partyIdentifications[i];
            partyIdentificationNumber =
                currPartyIdentification.identificationNumber;
            break;
        }

        //contact creation wrapper - starts
        this.contactData.GlobalPartyId =
            this.unifiedProfiles[unifiedprofileindex].globalPartyId;
        this.contactData.FirstName =
            this.unifiedProfiles[unifiedprofileindex].firstName;
        this.contactData.LastName =
            this.unifiedProfiles[unifiedprofileindex].lastName;
        this.contactData.Phone = latestHomePhoneNumber;
        this.contactData.Extension__c = latestExtensionNumber;
        this.contactData.MobilePhone = latestMobilePhoneNumber;
        this.contactData.Physician_Registration_No__c =
            partyIdentificationNumber;
        this.contactData.Email = emailAddressName;
        this.contactData.MailingStreet = currentMailingStreet;
        this.contactData.OtherStreet = currentOtherStreet;
        this.contactData.MailingCity = currentMailingCity;
        this.contactData.MailingPostalCode = currentPostalCode;
        //SFSC - 9860 - Mapped state and country code - starts
        this.contactData.MailingCountryCode = currentMailingCountryCode;
        this.contactData.MailingStateCode = currentMailingStateCode;
        //SFSC - 9860 - Mapped state and country code - ends
        //contact creation wrapper - ends
    }

    closeRecordTypeModal() {
        this.showRecordTypeModal = false;
        //Start: SFSC-4996 check if object selection window is visible to user or not
        if (this.leadCreateAccess && this.contactCreateAccess) {
            this.showObjectSelectionModal = true;
        }
        this.isRendered = false; //SFSC - 7070 reset flag
        //End: SFSC-4996
    }

    enterKeyCheck(event) {
        if (event.keyCode === 13) {
            this.checkInputValidation(); // Added on 3 Feb 2022
        } else {
            return false;
        }
    }
    handleRecordTypeNext(event) {
        var eventData = event.detail;
        this.contactData.contactCreateModal = eventData.contactCreateModal;
        this.contactData.selectedRecordTypeId = eventData.recordTypeId;
        this.contactData.selectedRecordTypeName = eventData.recordTypeName;
        this.contactData.contactRecordTypeValues =
            eventData.contactRecordTypeValues;
        this.showRecordTypeModal = false;
        this.showContactCreateModal = true;
    }
    /**
     *
     * @param {property}.CdpSearch.contactData: {
     * firstName: any;
     * lastName: any;
     * globalPartyId: any;
     * partyIdentificationNumber: any;
     * phone: any;
     * mobilePhone: any;
     * extension: any;email: any;
     * mailingStreet: any;
     * otherStreet: any;
     * mailingCity: any;
     * mailingPostalCode: any;
     * mailingCountry: any;
     * mailingState: any;
     * @event closeContactCreateModal
     */
    closeContactCreateModal(event) {
        this.showContactCreateModal = false;
        // get contact data from eventArgs
        this.contactData = event.detail;
        //SFSC - 7070  - starts
        this.showRecordTypeModal = false; // 01-02-7070 - Added when user click on cancel button of contact create page, record type page should not open
        this.isRendered = true; //set flag is Rendered as true
        //SFSC - 7070  - ends
        //Start:SFSC-4996
        if (this.contactCreateAccess && this.leadCreateAccess) {
            this.showObjectSelectionModal = true;
        }
        //End: SFSC-4996
    }

    checkForMultipleRecordTypeAccess() {
        let recordtypeinfo =
            this.contactObjectInfo &&
            this.contactObjectInfo.hasOwnProperty('data') &&
            this.contactObjectInfo.data
                ? this.contactObjectInfo.data.recordTypeInfos
                : null;

        if (recordtypeinfo) {
            let recordTypeDict = Object.values(recordtypeinfo);
            let sorted = [];

            /**
             * Returns the elements of an array that meet the condition specified in a callback function.
             * @param predicate — A function that accepts up to three arguments. The filter method calls the predicate function one time for each element in the array.
             * @param thisArg — An object to which the this keyword can refer in the predicate function. If thisArg is omitted, undefined is used as the this value.
             */
            recordTypeDict
                .filter(
                    (t) =>
                        t.defaultRecordTypeMapping === true &&
                        t.name !== 'Master' &&
                        t.available === true
                )
                .forEach((t) => sorted.push(t));
            recordTypeDict
                .filter(
                    (t) =>
                        t.defaultRecordTypeMapping === false &&
                        t.name !== 'Master' &&
                        t.available === true
                )
                .forEach((t) => sorted.push(t));

            sorted.forEach((t) => {
                if (t.defaultRecordTypeMapping === true) {
                    this.contactData.selectedRecordTypeId = t.recordTypeId;
                }
            });
            this.hasMultipleRecordTypeAccess = sorted.length > 1;
            if (this.isSearchFromButtonOverride) {
                this.CDPSearch = null;
                if (sorted.length > 1) {
                    //4977 changes
                    this.showRecordTypeModal = true;
                    this.showContactCreateModal = false;
                } else {
                    this.showContactCreateModal = true;
                    this.showRecordTypeModal = false;
                }
            }
        }
        this.isRendered = true; //SFSC - 7070 - set isRendered flag as true
    }

    //SFSC-4996 Code starts
    handleCancel() {
        this.showObjectSelectionModal = false;
        this.selectedObject = null;
    }
    openObjectSelectionModal(event) {
        this.unifiedprofileindex = event.target.dataset.unifiedprofileindex;
        if (this.contactCreateAccess && this.leadCreateAccess) {
            this.showObjectSelectionModal = true;
        } else if (this.contactCreateAccess) {
            this.navigateContactCreation();
        } else if (this.leadCreateAccess) {
            this.navigateToLeadCreation();
        }
    }
    get options() {
        return [
            {
                label: 'Contact',
                value: 'Contact',
                defaultRecordType: this.selectedObject === 'Contact'
            },
            {
                label: 'Lead',
                value: 'Lead',
                defaultRecordType: this.selectedObject === 'Lead'
            }
        ];
    }

    handleObjectSelection(evt) {
        this.selectedObject = evt.target.dataset.selectedobject;
    }

    navigateFromObjectModal() {
        if (this.selectedObject === 'Contact') {
            this.showObjectSelectionModal = false;
            this.navigateContactCreation();
        } else if (this.selectedObject === 'Lead') {
            this.navigateToLeadCreation();
        }
    }

    navigateToLeadCreation() {
        if (this.unifiedprofileindex !== undefined) {
            this.mapSelectedUnifiedProfileRecordData();
        }
        const defaultValues = encodeDefaultFieldValues({
            FirstName: this.contactData.FirstName,
            LastName: this.contactData.LastName,
            Email: this.contactData.Email,
            Phone: this.contactData.Phone,
            MobilePhone: this.contactData.MobilePhone,
            Street: this.contactData.MailingStreet,
            City: this.contactData.MailingCity,
            PostalCode: this.contactData.MailingPostalCode,
            CountryCode: this.contactData.MailingCountryCode, //SFSC - 9860 - Mapped country code
            StateCode: this.contactData.MailingStateCode, //SFSC - 9860 - Mapped State Code
            Extension__c: this.contactData.Extension__c,
            Physician_Registration_Number__c:
                this.contactData.Physician_Registration_No__c,
            GlobalPartyId: this.contactData.GlobalPartyId
        });

        this[NavigationMixin.Navigate]({
            type: 'standard__objectPage',
            attributes: {
                objectApiName: 'Lead',
                actionName: 'new'
            },
            state: {
                nooverride: '1',
                useRecordTypeCheck: '1',
                defaultFieldValues: defaultValues
            }
        });
    }

    get isObjectNextButtonDisabled() {
        return this.selectedObject == null || this.selectedObject === '';
    }
    //SFSC-4996 code ends
    // Added on 2 Feb 2022 - SFSC - 7070 - starts
    handleManualRendered() {
        this.isRendered = false;
        eval("$A.get('e.force:refreshView').fire();");
    }
    // Added on 2 Feb 2022 - SFSC - 7070 - ends
    //Added this method to check the object access for lead and contact
    checkObjectAccess() {
        if (this.contactObjectInfo.data !== undefined) {
            this.contactCreateAccess = JSON.parse(
                JSON.stringify(this.contactObjectInfo)
            ).data.createable;
        }
        if (this.leadObjectInfo.data !== undefined) {
            this.leadCreateAccess = JSON.parse(
                JSON.stringify(this.leadObjectInfo)
            ).data.createable;
        }
        if (!this.contactCreateAccess && !this.leadCreateAccess) {
            this.isCreateButtonAvailableToUser = false;
        }
    }
}
```

```html
<template>
    <lightning-card>
        <template if:false={isSearchFromButtonOverride}
            ><!--4977 changes-->
            <div class="slds-var-p-around_medium lgc-bg">
                <!-- SFSC-4536 Update placeholder value Start-->
                <div class="slds-combobox-group">
                    <lightning-combobox
                        placeholder="Search by…"
                        class="searchType slds-listbox_inline slds-size_1-of-5"
                        name="searchInput"
                        value={value}
                        onchange={handleChange}
                        options={searchOptions}
                        disabled={isSearchTypeDisabled}
                    ></lightning-combobox>
                    <!-- Updated on 24 Feb 2022 -->
                    <lightning-input
                        class="searchInput slds-listbox_inline slds-listbox__option-text_entity slds-size_4-of-5"
                        type="search"
                        placeholder="Please Enter a Phone or Email. Cannot Search Using Name"
                        onkeypress={enterKeyCheck}
                        onchange={handleSearchExpressionChange}
                        disabled={isSearchInputDisabled}
                    >
                    </lightning-input>
                </div>
                <br />
                <!-- Added on 3 Feb 2022 -  onclick={checkInputValidation} and REMOVED onclick={handleSearch}-->
                <lightning-button
                    variant="brand"
                    label="Search"
                    title="Search Global Profile"
                    onclick={checkInputValidation}
                    disabled={isSearchButton}
                ></lightning-button>

                <template if:true={displayUnifiedProfileTable}>
                    <div class="slds-grid slds-gutters">
                        <div class="slds-col">
                            <table
                                class="slds-table slds-table_cell-buffer slds-table_bordered slds-var-m-top_small"
                            >
                                <thead>
                                    <tr class="slds-line-height_reset">
                                        <th class="" scope="col">
                                            <div
                                                class="slds-truncate"
                                                title="Name"
                                            ></div>
                                        </th>
                                        <th class="" scope="col">
                                            <div
                                                class="slds-truncate"
                                                title="Name"
                                            >
                                                Name
                                            </div>
                                        </th>
                                        <th class="" scope="col">
                                            <div
                                                class="slds-truncate"
                                                title="Phone"
                                            >
                                                All Phones
                                            </div>
                                        </th>
                                        <th class="" scope="col">
                                            <div
                                                class="slds-truncate"
                                                title="Email"
                                            >
                                                All Emails
                                            </div>
                                        </th>
                                        <th class="" scope="col">
                                            <div
                                                class="slds-truncate"
                                                title="Address"
                                            >
                                                All Addresses
                                            </div>
                                        </th>
                                        <!--th scope="col">
    										<div class="slds-truncate" title="Source Record">
    											Source Record
    										</div>
    									</th-->
                                        <th class="" scope="col">
                                            <div
                                                class="slds-truncate"
                                                title="Source Names"
                                            >
                                                Source Names
                                            </div>
                                        </th>
                                        <th class="" scope="col">
                                            <!--//SFSC-8442 Added Active/Inactive Flag to 360 Search Results-->
                                            <div
                                                class="slds-truncate"
                                                title="Active Status"
                                            >
                                                Active Status
                                            </div>
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <template
                                        for:each={unifiedProfileCurrentPage}
                                        for:item="unifiedProfile"
                                        for:index="index"
                                    >
                                        <tr
                                            class="slds-hint-parent"
                                            key={unifiedProfile.globalPartyId}
                                        >
                                            <td>
                                                <div
                                                    style="
                                                        white-space: pre-wrap;
                                                    "
                                                    class="slds-truncate"
                                                >
                                                    <!--SFSC-4996 Added isCreateButtonAvailableToUser check for users who doesn't have access to contact and lead create. -->
                                                    <template
                                                        if:true={isCreateButtonAvailableToUser}
                                                    >
                                                        <template
                                                            if:true={unifiedProfile.canCreateContact}
                                                        >
                                                            <!--Changed onclick method name for SFSC-4996-->
                                                            <lightning-button
                                                                data-unifiedprofileindex={index}
                                                                label="Create New"
                                                                onclick={openObjectSelectionModal}
                                                            >
                                                            </lightning-button>
                                                        </template>
                                                    </template>
                                                </div>
                                            </td>
                                            <td class="wrapword">
                                                <div
                                                    style="
                                                        white-space: pre-wrap;
                                                    "
                                                    class="slds-truncate"
                                                >
                                                    <span class="wrapword"
                                                        >{unifiedProfile.firstName}
                                                        {unifiedProfile.lastName}</span
                                                    >
                                                </div>
                                            </td>
                                            <td>
                                                <div class="slds-truncate">
                                                    <template
                                                        for:each={unifiedProfile.phoneNumbers}
                                                        for:item="phoneNumber"
                                                    >
                                                        <span key={phoneNumber}
                                                            ><template
                                                                if:true={phoneNumber.isContactType}
                                                                >Phone:&nbsp;</template
                                                            ><template
                                                                if:false={phoneNumber.isContactType}
                                                                >Mobile:&nbsp;
                                                            </template>
                                                            {phoneNumber.telephoneNumber}&nbsp;
                                                            <template
                                                                if:true={phoneNumber.isContactType}
                                                            >
                                                                <template
                                                                    if:true={phoneNumber.extensionNumber}
                                                                >
                                                                    ext.
                                                                    {phoneNumber.extensionNumber}
                                                                </template>
                                                            </template>
                                                            <br
                                                        /></span>
                                                    </template>
                                                </div>
                                            </td>
                                            <td>
                                                <div
                                                    style="
                                                        white-space: pre-wrap;
                                                    "
                                                    class="slds-truncate"
                                                >
                                                    <template
                                                        for:each={unifiedProfile.contactPointEmails}
                                                        for:item="email"
                                                    >
                                                        <span key={email}
                                                            >{email.emailAddressName}
                                                            <br
                                                        /></span>
                                                    </template>
                                                </div>
                                            </td>
                                            <td class="wrapword">
                                                <div class="slds-truncate">
                                                    <template
                                                        for:each={unifiedProfile.fullAddress}
                                                        for:item="fullAddress"
                                                    >
                                                        <span
                                                            class="wrapword"
                                                            key={fullAddress.concatenatedAddress}
                                                            >{fullAddress.concatenatedAddress}
                                                            <br
                                                        /></span>
                                                    </template>
                                                </div>
                                            </td>
                                            <!--td>
    											<div class="slds-truncate">
    												<template for:each={unifiedProfile.dataSourcePoints}
    													for:item="dataSourcePoint">
    													<span key={dataSourcePoint}
    														>{dataSourcePoint.dataSourcerecordId}<br
    													/></span>
    												</template>
    											</div>
    										</td-->
                                            <td>
                                                <div
                                                    style="
                                                        white-space: pre-wrap;
                                                    "
                                                    class="slds-truncate"
                                                >
                                                    <template
                                                        for:each={unifiedProfile.dataSourcePoints}
                                                        for:item="dataSourcePoint"
                                                    >
                                                        <!--Changed to hyperlink - SFSC - 4977 - on clicking on data source name it should navigate-->
                                                        <template
                                                            if:true={dataSourcePoint.isSalesforceRecord}
                                                        >
                                                            <span
                                                                key={dataSourcePoint.dataSourcerecordId}
                                                                ><a
                                                                    onclick={navigateToRecord}
                                                                    title="Hello"
                                                                    data-datasourceindex={index}
                                                                    data-recordid={dataSourcePoint.dataSourcerecordId}
                                                                    target="_blank"
                                                                    >{dataSourcePoint.dataSourceName}</a
                                                                ><br
                                                            /></span>
                                                        </template>
                                                        <template
                                                            if:false={dataSourcePoint.isSalesforceRecord}
                                                        >
                                                            <span
                                                                key={dataSourcePoint.dataSourcerecordId}
                                                                >{dataSourcePoint.dataSourceName}<br
                                                            /></span>
                                                        </template>
                                                    </template>
                                                </div>
                                            </td>
                                            <td class="wrapword">
                                                <div
                                                    style="
                                                        white-space: pre-wrap;
                                                    "
                                                    class="slds-truncate"
                                                >
                                                    <span class="wrapword"
                                                        ><!--//SFSC-8442 Added Active/Inactive Flag to 360 Search Results-->
                                                        <template
                                                            if:true={unifiedProfile.isActive}
                                                        >
                                                            <lightning-input
                                                                class="slds-var-p-left_xx-large"
                                                                type="checkbox"
                                                                disabled
                                                                checked
                                                            ></lightning-input>
                                                        </template>
                                                        <template
                                                            if:false={unifiedProfile.isActive}
                                                        >
                                                            <lightning-input
                                                                class="slds-var-p-left_xx-large"
                                                                type="checkbox"
                                                                disabled
                                                                unchecked
                                                            ></lightning-input>
                                                        </template>
                                                    </span>
                                                </div>
                                            </td>
                                        </tr>
                                    </template>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </template>
                <div class="slds-var-m-around_large">
                    <div if:true={showSpinner} class="slds-is-relative">
                        <lightning-spinner
                            alternative-text="Loading..."
                            variant="brand"
                        >
                        </lightning-spinner>
                    </div>
                </div>
            </div>

            <template if:true={isResult}>
                <div
                    class="slds-var-p-top_xx-small slds-var-p-bottom_large slds-align_absolute-center"
                    data-aura-rendered-by="4:1004;a"
                >
                    <figure data-aura-rendered-by="5:1004;a">
                        <div
                            class="col-grid slds-grid_vertical slds-align_absolute-center"
                            data-aura-rendered-by="12:1004;a"
                        >
                            <h2
                                class="slds-text-heading_medium slds-text-align_center"
                            >
                                <!--SFSC-8029: Added on 05-12-2022 : Added additional message in case of no search results for mobile or phone-->
                                {additionalMessage}
                                <br />No results for {searchKey} in CDP.
                                <br />
                                Do you want to &nbsp;<a
                                    onclick={openObjectSelectionModal}
                                    >Create new record</a
                                >?
                            </h2>
                        </div>
                    </figure>
                </div>
            </template>
            <template if:true={isTimeOut}>
                <div
                    class="slds-var-p-top_medium slds-var-p-bottom_large slds-align_absolute-center"
                >
                    <figure>
                        <div
                            class="col-grid slds-grid_vertical slds-align_absolute-center"
                        >
                            <h2
                                class="slds-text-heading_medium slds-var-p-top_medium slds-text-align_center"
                            >
                                Session has timeout for {searchKey}.<br />
                                Do you want to &nbsp;<a
                                    onclick={openObjectSelectionModal}
                                    >Create new record</a
                                >?
                            </h2>
                        </div>
                    </figure>
                </div>
            </template>
            <div class="slds-var-m-around_medium">
                <template if:true={displayUnifiedProfileTable}>
                    <p class="slds-var-m-vertical_medium content">
                        Displaying Page {page} of {totalPage}.
                    </p>
                    <lightning-layout>
                        <lightning-layout-item>
                            <lightning-button
                                name="globalSearch-PreviousButton"
                                class="global-search-prev-btn"
                                label="Previous"
                                disabled={isFirstPage}
                                icon-name="utility:chevronleft"
                                data-field="previous"
                                onclick={previousHandler}
                            >
                            </lightning-button>
                        </lightning-layout-item>
                        <lightning-layout-item
                            flexibility="grow"
                        ></lightning-layout-item>
                        <lightning-layout-item>
                            <lightning-button
                                name="globalSearch-NextButton"
                                class="global-search-next-btn"
                                label="Next"
                                disabled={isLastPage}
                                icon-name="utility:chevronright"
                                icon-position="right"
                                data-field="next"
                                onclick={nextHandler}
                            >
                            </lightning-button>
                        </lightning-layout-item>
                    </lightning-layout>
                </template>
            </div>
            <!--SFSC-4996 code starts-->
            <template if:true={showObjectSelectionModal}>
                <section
                    role="dialog"
                    tabindex="-1"
                    aria-labelledby="modal-heading-01"
                    aria-modal="true"
                    aria-describedby="modal-content-id-1"
                    class="slds-modal slds-fade-in-open"
                >
                    <div class="slds-modal__container">
                        <header class="slds-modal__header">
                            <lightning-button-icon
                                class="slds-modal__close"
                                icon-name="utility:close"
                                icon-class="slds-button_icon-inverse"
                                title="Close"
                                onclick={handleCancel}
                            >
                            </lightning-button-icon>
                            <h2>
                                <strong>Please Select a Type of Record</strong>
                            </h2>
                        </header>
                        <div
                            class="slds-modal__content"
                            id="modal-content-id-1"
                        >
                            <div
                                class="slds-var-p-around_medium slds-form-element slds-form-element__control"
                            >
                                <template
                                    for:each={options}
                                    for:item="objectName"
                                    for:index="index"
                                >
                                    <lightning-input
                                        class="object-name"
                                        label={objectName.label}
                                        key={objectName.value}
                                        type="radio"
                                        value={objectName.value}
                                        data-selectedobject={objectName.value}
                                        checked={objectName.defaultRecordType}
                                        onchange={handleObjectSelection}
                                        name="searchObject"
                                    ></lightning-input>
                                </template>
                            </div>
                        </div>
                        <footer class="slds-modal__footer">
                            <lightning-button
                                class="slds-button"
                                label="Cancel"
                                variant="neutral"
                                onclick={handleCancel}
                            >
                            </lightning-button>
                            <lightning-button
                                class="slds-button"
                                label="Next"
                                variant="brand"
                                disabled={isObjectNextButtonDisabled}
                                onclick={navigateFromObjectModal}
                            ></lightning-button>
                        </footer>
                    </div>
                </section>
                <div class="slds-backdrop slds-backdrop_open"></div>
            </template>
        </template>
        <!--SFSC-4996 code ends-->
        <!--Record type slection page - starts-->
        <template if:true={showRecordTypeModal}>
            <c-cdp-contact-record-selection-page
                onrecordtypemodalnext={handleRecordTypeNext}
                is-global-search-page={CDPSearch}
                oncloserecordtypemodal={closeRecordTypeModal}
                contact-data={contactData}
                show-record-type-modal={showRecordTypeModal}
            ></c-cdp-contact-record-selection-page>
        </template>
        <!--Record type slection page - ends-->

        <!--Contact creation page - starts-->
        <template if:true={showContactCreateModal}>
            <c-cdp-contact-create-form
                record-type-id={contactData.selectedRecordTypeId}
                is-global-search-page={CDPSearch}
                has-multiple-record-type-access={hasMultipleRecordTypeAccess}
                show-contact-create-modal={showContactCreateModal}
                contact-data={contactData}
                onclosecontactcreatemodal={closeContactCreateModal}
                onmanualrendered={handleManualRendered}
            >
            </c-cdp-contact-create-form>
            <!-- Added on 2 Feb 2022 : onmanualrendered={handleManualRendered} - SFSC - 7070-->
        </template>
        <!--Contact creation page - ends-->
    </lightning-card>
</template>
```

```css
.green-color {
    --sds-c-icon-color-foreground-default: green;
    --sds-c-icon-color-foreground: green;
}
.red-color {
    --sds-c-icon-color-foreground-default: rgb(170, 8, 8);
    --sds-c-icon-color-foreground: rgb(170, 8, 8);
}
.gp-no-results {
    font-family: 'Salesforce Sans', Arial, sans-serif;
}
.wrapword {
    white-space: -moz-pre-wrap !important; /*Mozilla, since 1999*/
    white-space: -webkit-pre-wrap; /*Chrome and Safari */
    white-space: -pre-wrap; /*Opera 4-6*/
    white-space: -o-pre-wrap; /*Opera 7*/
    white-space: pre-wrap; /* css-3 */
    word-wrap: break-word; /*Internet Explorer 5.5+ */
    word-break: break-word;
    white-space: normal;
}

::-webkit-input-placeholder {
    color: red;
}
```
