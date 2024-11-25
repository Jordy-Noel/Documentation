# Financial dimensions

## How to pass a custom financial dimension tag filtered on a dimension attribute.

*CASE | A financial dimension is defined with custom dimension values. These values are referenced as the default dimension on CustInvoiceLine records. The aim is to retrieve the display value of these dimension values.*

### Approach via relations

1. Retrieve a custInvoiceLine record.
2. Retrieve a DimensionAttributeValueSet record based on the relation `custInvoiceLine.DefaultDimension == DimensionAttributeValueSet.RECID`
3. Retrieve DimensionAttributeValueSetItem record based on the relation `DimensionAttributeValueSet.RECID == DimensionAttributeValueSetItem.DimensionAttributeValueSet`
4. Retrieve a DimensionAttributeValue record based on the relation `DimensionAttributeVAlueSetItem.DimensionAttributeValue == DimensionAttribueValue.RECID`
5. Join a DimensionAttribute record based on the relation `DimensionAttributeValue.DimensionAttribute == DimensionAttribute.RECID`. Filter on the Attribute Name `DimensionAttribute.Name == <Value>` 
6. Retrieve the display value of the DimensionAttributeValue record.