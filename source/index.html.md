---
title: Smart Reference Data REST API

<!-- language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript --> 

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  
  - errors
 

search: true
---

# Overivew

The *Smart Reference Data REST API* provides ¨your¨ with a Convenient, and simple web services interface for interacting with Smart Reference Data database server. The REST APIs are designed to easily access Smart Reference Data database and perform key tasks, that include, insert, update, and retrieve operation.

The Intergraph Smart® Reference Data REST API Help explains the basics of using the REST APIs to access Smart Reference Data database server.

You will be able to perform insert, update, and retrieve operations on the following business objects:

* Projects
* Disciplines
* Nls
* Attributes
* Tables
* Geometrics
* Table Groups
* Table Details
* Commodity Rules
* Commodity Group
* Commodity Parts
* Commodity Codes
* Idents
* Company Idents
* Schedule Jobs

You will be able to perform retrieve operation on the following business objects:

* Specification Types
* Specification Headers
* Specification Header Details
* Specification Header Geometrics
   * Specification Header Notes
   * Specification Items
   * Specification Item Notes


# Authorization Architecture

A common OAuth allows a third-party client, such as PostMan web API, termed the client in the OAuth 2.0 specification, to operate on behalf of a user, without revealing that user’s credentials, such as user name and password, to the client. The client first sends the user credentials to an authorization server (Intergraph's Cloud9 service), which authenticates the user, obtains the user’s authorization, and issues an access token which the client can use in interacting with a resource server (Smart Reference Data database server)
![OAuth2.0](OAuth2.0.png)


`Authorization: Process Flow`

1. User’s API Client sends an access token request to the SAM server.
2. SAM server authenticates and responds with an access token.
3. User sends an API request along with the access token.
4. API server validates the access token.
5. API server sends the API request to the Smart Reference Data (SRD) Server.
6. SRD sends the response to the API Server.
7. API Server sends the response to the user's API client.





## Access Privileges

Access privileges allows you to perform read or write operations in Smart Reference Data using REST APIs. You must assign the following privileges for a user to access data in Smart Reference Data:

| Privilege          | Short Desc        | Description                 |
|--------------------|-------------------|-----------------------------|
| API_ATTRIBUTES_R   | View Attributes   | Allows to view attributes   |
| API_ATTRIBUTES_R/W | Create Attributes | Allows to create attributes |
| API_GEOMETRICS_R   | View Geometrics   | Allows to view geometrics   |
| API_GEOMETRICS_R/W | Create Geometrics | Allows to create geometrics |
| API_TABLES_R       | View Tables       | Allows to view tables       |
| API_TABLES_R/W     | Create Tables     | Allows to create tables     |

## Request access token

**Sampel URI**
`POST https://samdev.ingrnet.com/sam/oauth/connect/token`

<aside class="notice">
Make sure you select the x-www-form-urlencoded body format from your client.
</aside>

> The above command returns JSON structured like this:

```json
	{

  "access_token": "XXXXXXXX…",

  "expires_in": 86400,

  "token_type": "Bearer"

}
```

You must have the following key value pairs in the request Body to access the token:

| Key           | Value                         |
|---------------|-------------------------------|
| grant_type    | password                      |
| username      | user created in SAM           |
| password      | password for SAM user         |
| client_id     | client Id from SAM            |
| client_secret | client secret from SAM        |
| scope         | Smart API Service Id from SAM |

### HTTP Request


`POST https://samdev.ingrnet.com/sam/oauth/connect/token <id> test`

# API Discoverability

The following section helps you to find the available projects, disciplines, and the National Language Support (NLS) in Smart Reference Data database server.

**Standard URI format**

| Resource                  | Description                                            |
|---------------------------|--------------------------------------------------------|
| in-sprdapisrv.ingrnet.com | API Server where all the RESTFUL services are deployed |
| SRD713                    | Virtual directory on the API Server                    |
| Srd/V2                    | Route prefix                                           |
| client_id                 | client Id from SAM                                     |
| client_secret             | client secret from SAM                                 |
| scope                     | Smart API Service Id from SAM                          |

## Configure Web API

### Application settings

You must set the following configuration in the web.config file to consume from client service:

   * Open the web.config file, search for `Services baseUri`section, and set value for baseUri as:

`baseUri ="https://<ServerName>/<VirtualDirectoryName>"/>`

`https://in-sprdapisrv.ingrnet.com/<Server Name>/<Virtual Directory>/V2/Projects('SDB')/Disciplines(5020)/Nls(1)Tables`


For example, if your server name is in-sprdapisrv.ingrnet.com, and the virtual directory name is SRD713, the `<baseUri>` must be:

  `<services baseUri=" https://in-sprdapisrv.ingrnet.com/SRD713"/>`

## Nullable Values

* For a nullable column, if no value is passed in the request body, the software replaces the column with the default value 

if exists) else it is considered as NULL.

* For a mandatory column if no value is passed in the request body, the software replaces the column with the default value (if exists) else an appropriate message is shown.

# Manage Attributes
## Retrieve an Attribute
> The above command returns JSON structured like this:

```json
	 {

  "@odata.context": "https://in-sprdapisrv.ingrnet.com/SmartRDAPI/Srd/V2/$metadata#Projects('SDB')/Disciplines(5020)/Nls<SampleID> )/Attributes",

  "value": [

    {

      "AttributeId": 17979,

      "AttributeName": "EDIT_ATTRIBUTE",

      "AttributeTypeId": 5162,

      "AttributeGroupName": "STR ATTR",

      "Project": "SDB",

      "DataType": "CHAR",

      "Required": "ON",

      "FormWidth": 13,

      "DataWidth": 12,

      "Precision": null,

      "MetrEngl": "S.40.04.01",

      "UnitId": 5282,

      "Unit": "-"

    }

  ]

}

}
```
### Sample Request
`GET https://in-sprdapisrv.ingrnet.com/SmartRDAPI/Srd/V2/Projects('SDB')/Disciplines(5020)/Nls(1)/Attributes(<AttributeId>)`

<aside class="notice">If no (AttributeId) is passed, the API retrieves all the available attributes in the Smart Reference Data database server </aside>



Example URI to retrieve all the attributes:

` https://in-sprdapisrv.ingrnet.com/<Server Name>/<Virtual Directory>/V2/Projects('SDB')/Disciplines(5020)/Nls(1)/Attributes `

**Headers**

| Header name   | Description         | Required | Values                |
|---------------|---------------------|----------|-----------------------|
| Authorization | Access token        | Required | Bearer (access_token) |
| Content-Type  | Request type format | Required | application/json      |


**Get URI Parameter**

| Parameter   | Description                                             | Type    | Required | Notes |
|-------------|---------------------------------------------------------|---------|----------|-------|
| AttributeID | The attribute id to which you want to retrieve the data | Integer | Required |       |


**Response**

| Element            | Description                                                                                                 | Type    | Notes                                                                                                                                                                           |
|--------------------|-------------------------------------------------------------------------------------------------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AttributeId        | A unique ID for the attributeID of the attribute                                                            | Integer | Generated by the software                                                                                                                                                       |
| AttributeName      | Name of the new attribute                                                                                   | String  |                                                                                                                                                                                 |
| AttributeTypeId    |                                                                                                             | Integer | Generated by the software                                                                                                                                                       |
| AttributeGroupName | Identifies the attribute group which the current attribute is assigned to                                   | String  |                                                                                                                                                                                 |
| Project            | The project or product group where you want to insert the attribute                                         | String  |                                                                                                                                                                                 |
| DataType           | Attribute data type that you want to assign                                                                 | String  |                                                                                                                                                                                 |
| Required           | Specifies if the new attribute is mandatory                                                                 | String  | Default value is ON                                                                                                                                                             |
| FormWidth          | Size of display item in user interface                                                                      | Integer |                                                                                                                                                                                 |
| DataWidth          | Max data length of the field. The length depends on the physical column that the attribute is assigned to   | Integer | Default value is ‘CHAR’.Available data types are 'ALPHA', 'CHAR', 'INT', 'NUMBER', 'DATE', 'DATETIME', 'EDATE','JDATE', 'LONG', 'MONEY', 'RINT', 'RNUMBER', 'TIME', 'GRAPHICS'. |
| Precision          | Digits after decimal point. The precision depends on the physical column that the attribute is assigned to. | Integer |                                                                                                                                                                                 |
| MetrEngl           | Reference to conversion table                                                                               | String  | Default value is S.40.04.01                                                                                                                                                     |
| UnitId             |                                                                                                             | Integer |                                                                                                                                                                                 |
| Unit               | Lookup unit                                                                                                 | String  |                                                                                                                                                                                 |

#Manage Tables
##Retrieve Tables
### Sample URI request by table ID
`GET https://in-sprdapisrv.ingrnet.com/<Server Name>/<Virtual Directory>/V2/Projects('SDB')/Disciplines(5020)/Nls(1)Tables(<TableId>)`

> The command returns JSON structured like this:

```json
{

  "@odata.context": "https://in-sprdapisrv.ingrnet.com/SRD713/Srd/V2/$metadata#Projects('SDB')/Disciplines(5020)/Nls(1)/Tables",

  "value": [

    {

      "TableId": 6061,

      "Project": "SDB",

      "TableName": "M_A60_COUNT",

      "TableTypeId": 5021,

      "TableType": "PHYSICAL",

    }

  ]

}
```

<aside class="notice">
If no <TableId> is passed, the API retrieves all the available tables in the Smart Reference Data database server.
</aside>

Example URI to retrieve all the attributes:

` https://in-sprdapisrv.ingrnet.com/<Server Name>/<Virtual Directory>/V2/Projects('SDB')/Disciplines(5020)/Nls(1)Tables `

**Header**

| Header name   | Description         | Required | Values                  |
|---------------|---------------------|----------|-------------------------|
| Authorization | Access token        | Required | Bearer < access_token > |
| Content-Type  | Request type format | Required | application/json        |


**Get URI Parameter**



| Parameter | Description                                         | Type    | Required |
|-----------|-----------------------------------------------------|---------|----------|
| TableID   | The table id to which you want to retrieve the data | Integer | Required |

**Response**


| Element     | Description                                                       | Type    | Notes                     |
|-------------|-------------------------------------------------------------------|---------|---------------------------|
| TableId     | A unique ID for the table.                                        | Integer | Generated by the software |
| Project     | The project or product group from where the table is retrieved.   | String  |                           |
| TableName   | Name of the table                                                 | String  |                           |
| TableTypeId | A unique ID for the table type.                                   | Integer | Generated by the software |
| TableType   | Identifies the table type which the current table is assigned to. | String  |                           |


##Add Table##

**Header**

| Header name   | Description         | Required | Values                  |
|---------------|---------------------|----------|-------------------------|
| Authorization | Access token        | Required | Bearer < access_token > |
| Content-Type  | Request type format | Required | application/json        |

> The command returns JSON structured like this:

```json
{

 

      "TableName": "ADD_TABLE",

      "TableType": "PHYSICAL";

}
```
> 

**Post Body**

| Element   | Description                                                      | Type   | Notes    |
|-----------|------------------------------------------------------------------|--------|----------|
| TableName | Name of the new table that you want to create                    | String | Required |
| TableType | Identifies the table type which the current table is assigned to | String | Required |

**Sample request**

`POST https://in-sprdapisrv.ingrnet.com/<Server Name>/<Virtual Directory>/V2/Projects('SDB')/Disciplines(5020)/Nls(1)/Tables`

> The command returns JSON structured like this:

```json


{

  "@odata.context": "https://in-sprdapisrv.ingrnet.com/SRD713/Srd/V2/$metadata#Projects('SDB')/Disciplines(5020)/Nls(1)/Tables/$entity",

  "TableId": 11118,

  "Project": "SDB",

  "TableName": "ADD_TABLE",

  "TableTypeId": 5021,

  "TableType": "PHYSICAL",

```
> 


**Response**


| Element     | Description                                                     | Type    | Notes                     |
|-------------|-----------------------------------------------------------------|---------|---------------------------|
| TableId     | A unique ID for the table.                                      | Integer | Generated by the software |
| Project     | The project or product group from where the table is retrieved. | String  |                           |
| TableName   | Name of the table                                               | String  |                           |
| TableTypeId | A unique ID for the table type.                                 | Integer | Generated by the software |






