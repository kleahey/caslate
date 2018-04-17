# Applicant APIs

## Overview

The Applicant APIs allow a partner to search to see if an applicant has a Common Application account and if not, to pass existing student data along with the user in order to create a new Applicant account in the Common Applicant system. This will be a benefit to student users by allowing them to save time and more easily create a Common App account using the information they have already provided in the partner’s system.

## Data Requirements & Validations
### Field Lengths:
- <b>First, Middle, Last Names cannot be more than <u>50 characters</u></b>
- <b>Address lines 1,2,3 cannot be more than <u>60 characters</u></b>
- <b>City, State, County cannot be more than <u>30 characters</u></b>
- <b>Zip cannot be more than <u>10 characters</u></b>

### Email Validations:

Validation message | Validation
:--------------- | :-------
"Email should not be empty." | Is displayed when email is empty
"Email must include one @" | Is displayed when there is no @ in email
"Email is invalid" | Is displayed when there is no text after "@"
"Email cannot have more than 64 characters before the @" | Is displayed when there are more than 64 chars before "@"
"Email must contain at least one dot after @." | Is displayed when there is no dot after "@"
"Email must not contain dots in succession." | Is displayed when there are 2 dots in succession
"Email before and after @ cannot start or end with a dot." | Is displayed when email ends with the dot
"Email before and after @ cannot start or end with a dot." | Is displayed when email starts with the dot
"Email must not contain more than one @." | Is displayed when email contains two "@"
"Email has special characters we can't accept." | Is displayed is displayed when email contains unacceptable special characters
"Email has special characters we can't accept." | Is displayed is displayed when email contains unacceptable special characters in domain part
"Email is invalid." | Is displayed is displayed when there are more than 63 chars after dot
"Email is invalid." | Is displayed is displayed when there is 1 char after dot
"Email has special characters we can't accept." | Is displayed is displayed when email contains unacceptable special characters after dot
"Email should not be more than 254 chars" | Is displayed is displayed when email have more than 254 chars

###Address Validations:

- Validation message <b>"Country is invalid"</b> is displayed when invalid Country under Address is passed. <br/><b>For example:</b> Pass a country which doesn't exist.<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Min and Max length validation on Country under Address should be exactly 3
- Validation message <b>"State is Invalid"</b> is displayed when invalid State under Address is passed if country is US, Canada and China. <br/><b>For Example:</b> Pass a Canadian Province but Country is United States.
- Zip code is required without any specific format when country is China.

###Miscellaneous Validations
- Validation message <b>"Country Code is invalid"</b> when a CountryCode which doesn't exist is passed under phone.
- Leap year validation on Birthdate.<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Validation message should be displayed when "02/29/1999" birthdate is passed
- Start year validation matches the choice group<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Acceptable inputs should be 0,1,2<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Validation message updated to read "Start year" not StartYear<br/>
- Validate Application Type<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Validation message "Applicant Type is invalid" is displayed when Application type(FY/TR) is wrong.

### Applicant APIs:

1. Applicant Search
2. Applicant Creation

## Applicant Search

> JSON Schema to validate input parameters of getapplicant API (Pre Registration Check API)

```csharp
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "additionalProperties": false,
  "definitions": {},
  "id": "https://api.delta.devca.net/applicant/search/",
  "properties": {
    "birthdate": {
      "description": "Student Date of birth to validate if it can register for a Common Application Account",
      "id": "/properties/birthdate",
      "maxLength": 10,
      "minLength": 10,
      "pattern": "(0[1-9]|1[012])[- \\/.](0[1-9]|[12][0-9]|3[01])[- \\/.](19|20)\\d\\d",
      "title": "Student Date of Birth",
      "type": "string"
    },
    "email": {
      "description": "Email ID of Student/Candidate to check in Common Application",
      "id": "/properties/email",
      "minLength": 6,
      "maxLength": 254,
      "pattern": "^([a-zA-Z0-9_\\!\\#\\$\\%\\&\'\\*\/\\=\\?\\^_\\`\\{\\|\\}\\~\\-\\.?]+)@((([a-zA-Z0-9\\-]+\\.)+))([a-zA-Z]{2,63}|[0-9]{1,63})(\\]?)$",
      "title": "Student Email",
      "type": "string"
    }
  },
  "required": [
    "birthdate",
    "email"
  ],
  "type": "object"
}
```

> Applicant Search API - Mock

```csharp
{
  "birthdate": "09/26/1999",
  "email": "user@example.com"
}
```

> Request Data Format

```csharp
application/json
```

The applicant search allows partners to determine if a Common Application account already exists for a particular student based on their email address and date of birth.  If the email and date of birth match an existing account, the partner will receive the Common App ID of that account and a response as to whether the account exists in the current season or the previous.  If the email and date of birth do not match, the partner will receive a reply that no account was found.

The service will provide partners with the ability to determine if a student has an existing Common App account so that the student can be appropriately redirected to either the Common App login page or to complete the applicant creation process. Use of this check is required in order to use the applicant creation method; if it is ignored and an account exists, then the POST will fail upon attempting to use the applicant creation method.

It will also allow partners the ability to confirm an applicant created the Applicant Creation process once they were handed off to the redirect page.

Mock API Postman Collection: [OpenAPI.Register-alpha-swagger-postman.json](/json/OpenAPI.Register-alpha-swagger-postman-applicant-search.json)

Mock API End Point: [Register Applicant Mock API](https://api.delta.devca.net/applicant/)

## Applicant Creation

> JSON Schema for Applicant Registration:

```csharp
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "additionalProperties": false,
  "definitions": {},
  "id": "https://api.delta.devca.net/applicant/",
  "properties": {
    "address": {
      "additionalProperties": false,
      "id": "/properties/address",
      "properties": {
        "addressline1": {
          "description": "Line 1 of the student address.",
          "id": "/properties/address/properties/addressline1",
          "minLength": 1,
          "maxLength": 60,
          "title": "Address line1 schema",
          "type": "string"
        },
        "addressline2": {
          "description": "Line 1 of the student address (optional).",
          "id": "/properties/address/properties/addressline2",
          "maxLength": 60,
          "title": "Address line2 schema",
          "type": "string"
        },
        "addressline3": {
          "description": "Line 3 of the student address (optional).",
          "id": "/properties/address/properties/addressline3",
          "maxLength": 60,
          "title": "Address line3 schema",
          "type": "string"
        },
        "city": {
          "description": "City of student address.",
          "id": "/properties/address/properties/city",
            "maxLength": 30,
          "title": "The city schema",
          "type": "string"
        },
        "countrycode": {
          "description": "ISO Alpha - 3 Country Code",
          "id": "/properties/address/properties/countrycode",
          "maxLength": 3,
          "minLength": 3,
          "title": "Country Code schema",
          "type": "string"
        },
        "state": {
          "description": "Student State.",
          "id": "/properties/address/properties/state",
          "title": "The state schema",
          "type": "string"
        },
        "zip": {
          "description": "Zip/Postal Code of the address.",
          "id": "/properties/address/properties/zip",
          "minLength": 5,
          "maxLength": 10,
          "title": "The zip schema",
          "type": "string"
        }
      },
      "required": [
        "addressline1",
        "state",
        "countrycode",
        "zip",
        "city"
      ],
      "type": "object"
    },
    "applicanttype": {
      "description": "Type of Applicant to Register: First Year/Transfer.",
      "enum": [
        "FY",
        "TR"
      ],
      "id": "/properties/applicanttype",
      "maxLength": 2,
      "minLength": 2,
      "title": "Applicant type",
      "type": "string"
    },
    "birthdate": {
      "description": "Date of birth of student.",
      "id": "/properties/birthdate",
      "maxLength": 10,
      "minLength": 10,
      "pattern": "(0[1-9]|1[012])[- \\/.](0[1-9]|[12][0-9]|3[01])[- \\/.](19|20)\\d\\d",
      "title": "The birthdate schema",
      "type": "string"
    },
    "email": {
      "description": "Student Email Address.",
      "id": "/properties/email",
      "minLength": 5,
      "maxLength": 254,
      "pattern": "^([a-zA-Z0-9_\\!\\#\\$\\%\\&\\'\\*\\/\\=\\?\\^\\_\\`\\{\\|\\}\\~\\-\\.?]+)@((([a-zA-Z0-9\\-]+\\.)+))([a-zA-Z]{2,63}|[0-9]{1,63})(\\]?)$",
      "title": "The email schema",
      "type": "string"
    },
    "firstname": {
      "description": "Student First name",
      "id": "/properties/firstname",
      "maxLength": 25,
      "title": "First name schema",
      "type": "string"
    },
    "lastname": {
      "description": "Student Last name",
      "id": "/properties/lastname",
      "maxLength": 30,
      "title": "Last name schema",
      "type": "string"
    },
    "phone": {
      "id": "/properties/phone",
      "properties": {
        "countrycode": {
          "description": "Country Code for Phone number",
          "id": "/properties/phone/properties/countrycode",
          "title": "The countrycode schema",
          "type": "string"
        },
        "number": {
          "description": "Student Phone Number",
          "id": "/properties/phone/properties/number",
          "title": "The number schema",
            "maxLength": 21,
          "type": "string"
        }
      },
      "required": [
        "countrycode",
        "number"
      ],
      "type": "object"
    },
    "registrationtype": {
      "default": "STUDENT",
      "description": "Registration type, currently STUDENT by default. Reflects options displayed on first screen of Registration Screen",
      "enum": [
        "STUDENT"
      ],
      "id": "/properties/registrationtype",
      "title": "User Registration type",
      "type": "string"
    },
    "startyear": {
      "description": "Session Start Year Value for Applicant, reflects values displayed during registration for - When do you plan to start college?",
      "id": "/properties/startyear",
      "title": "Session Start Year Schema",
      "type": "string"
    }
  },
  "required": [
    "applicanttype",
    "email",
    "address",
    "phone",
    "birthdate",
    "lastname",
    "firstname"
  ],
  "type": "object"
}
```

> Mock API Request Data Sample

```csharp
{
  "address" : {
    "addressline1" : "Address line 1",
    "addressline2" : "foo",
    "addressline3" : "foo",
    "city" : "City",
    "countrycode" : "USA",
    "state" : "VA",
    "zip" : "30001"
  },
  "applicanttype" : "FY",
  "birthdate" : "09/26/1985",
  "email" : "foo@f.com",
  "firstname" : "Ravish",
  "lastname" : "Tiwari",
  "phone" : {
    "countrycode" : "+91",
    "number" : "7838060214"
  },
  "startyear" : "2018"
}
```

> Request file data format

```csharp
application/json
```

The Common App applicant creation process will be completed with a separate method allowing the partner to POST the actual registration data.

The data elements that must be sent to complete the applicant creation process are:

* Email Address
* First Name
* Last Name
* Address
* Phone Number
* Date of Birth
* Applicant Type (first year or transfer)

Requirements (field lengths, valid value lists, etc) for each data element are available here.

Mock API Postman Collection: [OpenAPI.Register-alpha-swagger-postman.json](/json/OpenAPI.Register-alpha-swagger-postman-applicant-creation.json)

Mock API End Point: [Register Applicant Mock API](https://api.delta.devca.net/applicant/)

## Recommended Implementation

The recommended implementation is to offer the student one action, then utilize the student account check web service to programmatically determine if the student should be sent to the Common App login screen or to the Common App applicant creation process.

![alt text](/images/account_creation.png)
