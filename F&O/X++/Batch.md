# Batch development

## Class info

- Extends Runbase batch
    - Extends Runbase

## Methods to override

- Caption
    - Returns the description from the current batch class by calling the static description method on the class if there is no one.

- Pack
    - Return the current batch parameters by packing them in a container variable.
- Unpack
    - Retrieve the batch parameters from SQL by unpacking the container in which they were saved.

- Dialog
    - Setup a dialog screen
        - Add the required fields etc in this logic.
- GetFromDialog
    - Read the values from the dialog fields into the class fields.
- DialogPostRun
    - Execute logic that needs to be executed after the dialog has been created:
        - call super();
        - Add event handlers for lookup & validation.
            ```c#
            batchFieldControl += eventhandler(batchFieldControl.OnValidated)
            ```
        - The onLookup method syntax can be copied from any form control.
- canRunInNewSession
    - Method that defines wether we start a new session to execute the batch logig.
        - set to -> 'return false'

## Calling the batch class

1. Create a menu item action
    - Select the correct object type -> Class
2. Create a menu extension in the correct F&O Module.
    - Select the correct menu item type
    - Select the newly created menu item action as the menu item name

## Method implementation

### OnLookup

For the OnLookUp implementation we want to create the following logic:
1. Initialize a sysReferenceTableLookup object using the formControl & tableNum of the table as parameters
     ```c#
    SysReferenceTableLookup::newParameters(tableNum(Table1), formControl);
    ```
2. We want to pass a query to this lookup variable. This query is a [SysQuery](SysQuery.md)
    ```c#
    sysTableLookup.parmQuery(query);
    ```
3. Thirdly we want to add lookup fields for the fields we want to show in the dropdown
    ```c#
    sysTableLookup.addLookupField(fieldNum(TableName,FieldName));
    ```
4. Lastly we want to return the result of the sysTableLookup performLookup method.
    ```c#
    return sysTableLookup.performLookup();
    ```





