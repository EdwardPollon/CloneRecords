Salesforce has limited cloning functionality when it comes to cloning related records simultaneously. This is designed to be a reusable Apex action that can be plugged into virtually any Flow to clone any parent record and any child records. 
All you need to do is pass in the record Id of the parent record and the object names of the parent and child records you're wanting to clone.

In order for this code to work, the relationship field that links the child records to the parent records must be the same across all objects and must correspond exactly to the parent object name, which should be input through the 'parentSObjectName' input variable.
For eample, if the parent object is Opportunity, the lookup/master-detail field on all child custom objects must be have an API name of 'Opportunity__c'.
The code will automatically work for all standard objects with relationships to the parent object if the parent object is standard as well.

Relationship Field Naming Convention Key:
Parent Standard Object -- Child Standard Object ----> lookup field on child object: [parent object API name]Id
Parent Standard Object -- Child Custom Object ------> lookup field on child object: [parent object API name]
Parent Custom Object -- Child Standard Object ------> lookup field on child object: [parent object API name]
arent Custom Object -- Child Custom Object --------> lookup field on child object: [parent object API name]

Create an invocable action from Flows that will allow you to clone records with ease. The code is designed to take three input variables from a Flow:

1) selectedSObjects --> This should be a semicolon separated list of object API names of the child related records you're wanting to clone (Example --> StandardObject1;CustomObject1__c;CustomObject2__c;StandardObject2 ).
   The order of the objects shouldn't matter. I've found that using a checkbox group from a screen flow passes in a semicolon separated list. If you want to pass a different kind of list to the code, such as a comma-separated list, you can modify
   line 48 of the code to split up your String using whichever character you like.
3) currentRecordId --> Pass the record Id of the current parent record you're working with. It's used to query the current record in the code and all of it's fields so that it can easily be cloned.
4) parentSObjectName --> Pass the API Name of the parent record you're cloning

The code will also spit out the record Id of the newly cloned parent record back to the Flow, which you can use to create a redirect link to the new record for easy access to your users.
