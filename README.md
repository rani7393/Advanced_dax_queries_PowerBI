# Advanced_dax_queries_PowerBI
📘 DAX Filter Context & Table Functions — Complete Reference

A consolidated reference guide for DAX functions that manipulate filter context, evaluate relationships, and shape tables in Power BI.

## 🧩 1. Filter Context Removal & Override Functions
### 1.1 ALL()

Removes filters from the referenced table or columns. Returns all rows.
Used for:

% of total

Overriding slicers and context

Creating grand totals

### 1.2 REMOVEFILTERS()

Removes filters without returning a table.
Improves readability when clearing filter context in a measure.

Use Case: Clean denominator calculations.

### 1.3 ALLSELECTED()

Removes filters inside the visual but respects slicers, page filters, and external selections.

Used for:

Interactive totals

Dynamic % of total based on user selections

### 1.4 KEEPFILTERS()

Adds filters in addition to the existing context instead of overriding it.

Useful when:

You want to apply an additional filter but still respect the current context

Preventing values from appearing when the required context isn't applied

### 1.5 ALLEXCEPT()

Removes all filters except those explicitly specified.

Use when:

You want totals while preserving specific grouping fields (e.g., Product Category)

## 🧩 2. Relationship & Filter Propagation Functions
### 2.1 USERELATIONSHIP()

Activates an inactive relationship during measure calculation.

Since Power BI can't use multiple active relationships between two tables, this function is critical for switching context.

Example:
A table has:

Active relationship on OrderDate

Inactive relationship on ShipDate

You can activate the ShipDate relationship using:
CALCULATE([Sales], USERELATIONSHIP(Date[Date], Sales[ShipDate]))

### 2.2 CROSSFILTER()

Overwrites the direction or behavior of relationship filtering in a calculation.

Allows you to specify:

One-way filtering

Bidirectional filtering

Turning a relationship off

Syntax:

CROSSFILTER(SourceColumn, TargetColumn, Direction)


Directions:

NONE → Disable relationship

ONEWAY → Filter direction from first to second

BOTH → Enable bidirectional filtering

Used for:

Adjusting context flow inside a single measure

Overriding model-level relationship settings

### 2.3 RELATED()

Retrieves a column value from the one-side of a relationship when used on the many-side.

### 2.4 RELATEDTABLE()

Returns matching rows from the many-side related to the current one-side row.
Equivalent to CALCULATETABLE() with implicit relationship filters.

### 2.5 LOOKUPVALUE()

Performs value lookups using criteria, with or without defined relationships.
Supports multiple search conditions.

## 🧩 3. Table Values & Distinct Functions
### 3.1 VALUES()

Returns a distinct list of values from a column or table, respecting current filter context.

### 3.2 DISTINCT()

Similar to VALUES() but handles blanks differently and is less context-sensitive.

### 3.3 VALUE()

Converts text to numeric values when possible.

### 3.4 SELECTEDVALUE()

Returns a single value if the column contains only one visible value; otherwise returns a default.

## 🧩 4. Table-Shaping Functions
### 4.1 SUMMARIZE()

Creates a summary table grouped by one or more columns.

### 4.2 ROW()

Returns a single-row, multi-column table.
Often used with GENERATE and GENERATESERIES.

### 4.3 DATATABLE()

Creates a static table defined entirely within DAX.

### 4.4 GENERATESERIES()

Creates a single-column table with sequential numbers.

### 4.5 CROSSJOIN()

Returns the Cartesian product of two or more tables.

### 4.6 INTERSECT()

Returns common rows between two tables.
Used for tasks like identifying returning customers.

### 4.7 EXCEPT()

Returns rows from Table A that do not appear in Table B.
Used for finding new customers, lost customers, etc.

## 🧩 5. Filter Context Injection Function
### 5.1 TREATAS()

Transfers (or "treats") a table of values as if they were filters on unrelated tables or columns.

This function is extremely powerful and acts like creating a virtual relationship.

Use Case Examples:

Mapping slicer selections from one table to another unrelated table

Applying filters across tables with no physical relationship

Handling many-to-many scenarios

Example:

CALCULATE(
    [Total Sales],
    TREATAS(VALUES('SlicerTable'[ProductID]), 'Sales'[ProductID])
)


This behaves like the two ProductID columns are related, even when no relationship exists.

## 🧩 6. Filter Comparison Functions
### 6.1 INTERSECT()

Returns rows common to both tables.

### 6.2 EXCEPT()

Returns rows in the first table but not the second.
