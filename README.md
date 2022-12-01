# OData Binding in Microsoft Dynamics 365 to create/update records using Boomi

## Problem Statement:
When we pass values to Lookup/ID fields in Microsoft Dynamics 365 (D365) to perform Create or Update operation, we can encounter the below error.

**[400] (0x0) CRM do not support direct update of Entity Reference properties, Use Navigation properties instead. [HTTP/1.1 400 Bad Request]**

Below are few fields and their respective objects which can throw the above error.

| Object        | Field         | 
| ------------- |-------------| 
| Account       | _new_emergencycontact4_value| 
| Contact     | _ownerid_value     | 
| Contact     | _parentcustomerid_value     |
| Task | _regardingobjectid_value     |  

## Resolution:
OData Binding needs to be performed by adding new fields to the profile. This binding maintains the Primary Key - Foreign Key relationship between D365 objects. 

**Step-1:** Find the Schema Name of the field to be mapped from D365 settings. 

**Step-2:** Add OData Binding @odata.bind in the field’s name. For some fields, _<parent-object-name> in singular needs to be appended in the name, e.g., regardingobjectid can be an account or a contact, so for an account regardingobjectid should be appended with _account. Upper-case/Lower-case alphabets need to be taken care of, along with trial and error approach to formulate the key.
  
| Object        | Field       | Binding Field      | Sample Value    |
| ------------- |-------------|--------------------|-----------------|
| Account       | _new_emergencycontact4_value| new_EmergencyContact4@odata.bind | /contacts(GUID Value)|
| Contact     | _ownerid_value     | ownerid@odata.bind | /systemusers(GUID Value) |
| Contact     | _parentcustomerid_value     |ParentCustomerId@odata.bind | /accounts(GUID Value) |
| Task | _regardingobjectid_value     |  regardingobjectid_account@odata.bind | /accounts(GUID Value)


If GUID Value is **xxxx-yyyy-0123-asdf**, then the OData Binding field’s value should be prepended with **/contacts(** and appended with **)**.
The prepended value containing object name should be in plural, e.g., contacts/accounts/etc. 

**Step-3:** Add the new fields in the JSON profile, by adding an Object Entry and updating the name in the Element Details, as per below screenshots.

![image](https://user-images.githubusercontent.com/12267939/177551560-77ecc058-99f3-420a-9724-0278a393dbde.png)

![image](https://user-images.githubusercontent.com/12267939/177551607-a982e265-e645-4007-bb2c-c224271da069.png)



