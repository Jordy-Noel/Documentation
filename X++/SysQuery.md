# SysQuery development

[Documentation](https://community.dynamics.com/blogs/post/?postid=1191fb3a-6309-4a11-8000-ab424bae519a)

## Creating a query

1. Start by creating a query object
```c#
Query qry = new Query();
```
2. Add a datasource object. This is the main table we want to perform the query on. This logic returns a QueryBuildDataSource object.
```c#
QueryBuildDataSource qbds = qry.AddDataSource(tableNum(TableName));
```
3. Now that we have added a datasource/table we can add ranges to filter the data. This method returns a QueryBuildRange object. Which we can then add a value to to filter on the table.
```c#
//  Seperate definition of range 
QueryBuildRange qbr = qbds.addRange(fieldNum(tableName, fieldName));
qbr.value(queryValue(FilterValue));
// define range and add a value inline.
qbds.addRange(fieldNum(tableName, fieldName)).value(queryvalue(Filtervalue));
```