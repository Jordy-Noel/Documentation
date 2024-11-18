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
- canRunInNewSession
    - Method that defines wether we start a new session to execute the batch logig.
        - set to -> 'return false'

## Calling the batch class

1. Create a menu item action
    - Select the correct object type -> Class
2. Create a menu extension in the correct F&O Module.
    - Select the correct menu item type
    - Select the newly created menu item action as the menu item name
