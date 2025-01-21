```
/*
|||||  Ledger account combinations	|||||
=========================================
*/

-- Dimension attributes
/*
	A dimension attribute represents an additional piece of classifying information that a user want to associate with a ledger combination. Represents classes of things.
*/
select * from DIMENSIONATTRIBUTE -- A custom view 

-- Dimension attribute values
/*
	A dimension attribute value is a specific instance of a dimension (DimensionAttribute)
	The table is a link between the DimensionAttribute record and the specified RECID value of the record that is reference on the dimension attribute record.
	Example: In the dimensionattribute table we reference the main account entity as an Attribute. Then our dimensionAttributeValue record will contain the RECID to the specific Main account record but we 
	also need the dimensionAttribute to find the backing Entity table (Main account).
*/
select * from DIMENSIONATTRIBUTEVALUE

-- Dimension attribute value combinations
/*
	It stores a full multisegment account combination. Together with some denormalized information about the combination.
	It stores
		- the concatenated sgements as a single string
		- a foreign key reference to the account structure
		- a foreign key to the main account that was used.
*/
select * from DIMENSIONATTRIBUTEVALUECOMBINATION

-- Dimension attribute value group combination 
/*
	To link the group of account structure values and segment values to the main record in the DimensionAttributeValueCombination table a record is inserted into the DimensionAttributeValueGroupCombination
*/
select * from DIMENSIONATTRIBUTEVALUEGROUPCOMBINATION 

-- Dimension attribute value group 
/*
	It stores each related group of segments that is associated with each structure that is present in the combination. By being referenced by the DimensionAttributelevel value records (DimensionAttributeValueGroup column)

*/
select * from DIMENSIONATTRIBUTEVALUEGROUP

-- Dimension attribute level value
/*
		It stores the individual segment values for each segment that is in the associated group (DimensionAttributeValueGroup) or structure. One record exists for each segment.
*/
select * from DIMENSIONATTRIBUTELEVELVALUE
```