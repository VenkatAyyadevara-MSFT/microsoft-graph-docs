# Microsoft Graph release notes and known issues

|  ![](./images/GitHub-Mark-64px.png) | **Contribute to this content.** <br /> Use GitHub to [suggest changes](https://github.com/OfficeDev/microsoft-graph-docs).  |
|---|:---|

This article provides information about the new features for developers that are available in the November 2015 release of the Microsoft Graph API, and any known issues that you might want to be aware of. 


## GA features in Microsoft Graph API

The following Microsoft Graph API features are generally available:

* Users
* Groups
* Files
* Mail
* Calendar
* Personal Contacts 
* Create, read, update, and delete (CRUD) operations
* Cross-origin resource sharing (CORS) support.

	
## Preview features in Microsoft Graph API

The following Microsoft Graph API preview features are available:

* Notes 
* Tasks
* Excel
* People
* Organizational Contacts
* Applications
* Service Principals
* Directory schema extensions
* Webhooks
* Insights and relationships: Trending Around and Working With
* Groups instant access to content after creation
* Converged authentication model for personal accounts as well as work and school accounts. Although this is a preview feature, it is available on both the `/v1.0` and `/beta` versions.


## Microsoft Graph known issues

The following are known issues with the Microsoft Graph.

### Users
#### No instant access after creation
Users can be created immediately through a POST on the user entity. An Office 365 license must first be assigned to a user, in order to get access to Office 365 services. Even then, due to the distributed nature of the service, it might take 15 minutes before files, messages and events entities are available for use for this user, through the Microsoft Graph API. During this time, apps will receive a 404 HTTP error response. 

#### Photo restrictions
* Reading (GET) and updating (PATCH) a user's profile photo _on v1.0_ [fail](#PhotoIssuesAll).
* Reading and updating a user's profile photo is only possible if the user has a mailbox.  Additionally, any photos that *may* have been previously stored using the **thumbnailPhoto** property (using the Office 365 unified API preview, or the Azure AD Graph, or through AD Connect synchronization) will no longer be accessible through the Microsoft Graph user photo property.  Failure to read or update a photo, in this case, would result in the following error:

```javascript
	{
	  "error": {
	    "code": "ErrorNonExistentMailbox",
	    "message": "The SMTP address has no mailbox associated with it."
	  }
	}
```

 > **NOTE**:  Shortly after GA, storage and retrieval of user profile photos will be enabled, even if the user does not have a mailbox, and this error should disappear.

### Groups
#### Policy
Using Microsoft Graph to create and name a unified group bypasses any unified group policies that are configured through Outlook Web App. 

#### Group permission scopes
The Microsoft Graph exposes two permission scopes (*Group.Read.All* and *Group.ReadWrite.All*) for access to groups APIs.  These permission scopes must be consented to by an administrator (which is a change from preview).  In the future we plan to add new scopes for groups that can be consented by users.

####Updating group photo
Updating (PATCH) a group's [profile photo] _on v1.0_ [fails](#PhotoIssuesAll). 

### Contacts
* Only personal contacts are currently supported. Organizational contacts are not currently supported in `/v1.0`, but can be found in `/beta`.
* Personal contact's mobile phone isn’t being returned for a contact. It will be added shortly. In the meantime, it can be accessed through Outlook APIs.

<a name="PhotoIssuesAll"></a>
### Photos for users, groups, and contacts
Certain scenarios in reading (GET) and updating (PATCH) a [profile photo](../../api-reference/v1.0/resources/profilephoto.md) for a user or group are not fully supported.
#### Get Photo
* User:  On v1.0, GET photo fails, even if the app is granted user.read or any other user.* permission. On the beta endpoint, it works as expected. 
* Contact and group:  GET photo works with the appropriate contact or group permissions. 

#### Updating Photo
* User and group: On v1.0, PATCH photo fails irrespective of the permissions granted to the app.  On the beta endpoint, it works as expected. 
* Contact:  PATCH photo works with the appropriate contact permission.

A change is being rolled out over the next few weeks that will enable full user photo GET and PATCH on v1.0.


### Drives, files and content streaming
* First time access to a user's personal drive through the Microsoft Graph before the user accesses their personal site through a browser leads to a 401 response.
* Upload and download of files (files in Office groups, drives, or mail file attachments) is limited to 4 MB.

### Functionality available only in existing Office 365 REST APIs
#### Synchronization
Outlook, OneDrive and Azure AD synchronization capabilities (in Azure AD this is also known as differential query) are not available in `/v1.0` or `/beta`.  If your application requires synchronization capabilities, please continue to use the existing Office 365 and Azure AD REST APIs, or explore the new webhooks preview feature offered through Microsoft Graph for events, messages and contacts.

> **NOTE**:  It is a goal to close the gap between the existing APIs and Microsoft Graph as quickly as possible, including synchronization.

#### Batching
Batching is not supported by Microsoft Graph. You can, however, use the Outlook beta endpoint and 
[batch Outlook REST calls](https://msdn.microsoft.com/en-us/office/office365/api/batch-outlook-rest-requests). 

#### Availability in China
The Microsoft Graph service is being rolled out to China, but is not available yet.

#### Service actions and functions
`isMemberOf` and `getObjectsById` are not available in Microsoft Graph

### Microsoft Graph permissions
Please review the [permission scopes topic](http://graph.microsoft.io/docs/authorization/permission_scopes) for the latest details on Microsoft Graph supported application and delegated permissions.  In addition, there are the following limitations for `v1.0`:

|Permission |	Permission type | Limitation |	Alternative |
|-----------|-----------------|------------|--------------|
|_User.ReadWrite_| Delegated	| Cannot update mobile phone number|	Also select `Directory.AccessAsUser.All`| 
|_User.ReadWrite.All_|	Delegated|	Cannot perform any CRUD operations on `User` other than updating user HD photo and extended profile properties|	Also select `Directory.ReadWrite.All` or `Directory.AccessAsUser.All` if user deletion is required.|
|_User.Read.All_|	Application	|Cannot perform any read operations on other users|	Also select `Directory.Read.All`|
| _User.ReadWrite.All_ |	Application |	Cannot perform any CRUD operations on `User` other than updating user HD photo and extended profile properties |	Also select`Directory.ReadWrite.All`. **NOTE**: User deletion will not be possible.|
|_Group.Read.All_	| Application |	Cannot enumerate groups or group memberships.  Can still read group content for Office groups	| Also select `Directory.Read.All` |
|_Group.ReadWrite.All_	| Application	| Cannot enumerate groups or group memberships, create groups, update group memberships or delete groups.  Can still read and update group content for Office groups.	| Also select `Directory.ReadWrite.All`. **NOTE**:  Group deletion will not be possible.|

Additionally there are the following `/beta` limitations:

|Permission |	Permission type | Limitation |	Alternative |
|-----------|-----------------|------------|--------------|
| _Group.ReadWrite.All_	| Delegated	| Cannot read or update planner tasks in Office groups	| Also select `Tasks.ReadWrite`|
|_Tasks.ReadWrite_	| Delegated	| Cannot read or update signed-in user's tasks| Also select `Group.ReadWrite.All`|

### OData related limitations
* **$expand** limitations: 
 * No support for `nextLink`
 * No support for more than 1 level of expand
 * No support with extra parameters (**$filter**, **$select**)
* Multiple namespaces are not supported
* GETs on `$ref` and casting is not supported on users, groups, devices, service principals and applications.
* `@odata.bind` is not supported.  This means that developers won’t be able to properly set the `Accepted` or `RejectedSenders` on a group.
* `@odata.id` is not present on non-containment navigations (like messages) when using minimal metadata
* Cross-workload filtering/search is not available. 
* Full-text search (using **$search**) is only available for some entities, like messages.

  >  Your feedback is important to us. Connect with us on [Stack Overflow](http://stackoverflow.com/questions/tagged/office365). Tag your questions with [MicrosoftGraph] and [office365].

  
             .

