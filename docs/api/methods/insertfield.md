---
title: InsertField
parent: Methods
grand_parent: API
nav_order: 16
---

# InsertField
{: .d-inline-block }

New
{: .label .label-purple }

Inserts a new field, at the specified position, in the imported CSV data.
{: .fs-4 .fw-300 }

---

## Syntax

*expression*.`InsertField`*(aIndex, \[FieldName\])*

### Parameters

<table>
<thead>
<tr>
<th style="text-align: left;">Part</th>
<th style="text-align: left;">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align: left;"><em>aIndex</em></td>
<td style="text-align: left;">Required. Identifier specifying a <code>Long</code> Type variable. Represents the index in which the new field will be inserted.</td>
</tr>
<tr>
<td style="text-align: left;"><em>FieldName</em></td>
<td style="text-align: left;">Optional. Identifier specifying a <code>String</code> Type variable representing the name of the new field.</td>
</tr>
</tbody>
</table>

### Returns value

*Type*: `CSVinterface`

---

See also
: [ImportFromCSV method](https://ws-garcia.github.io/VBA-CSV-interface/api/methods/importfromcsv.html), [ImportFromCSVstring method](https://ws-garcia.github.io/VBA-CSV-interface/api/methods/importfromcsvstring.html).

## Behavior

The `InsertField` method will insert a new field into all records, if they all have the same number of fields, in the current instance. The value of the `FieldName` parameter will be inserted into the record/first row, otherwise not.

### ☕Example

```vb
Sub InsertField()
    Dim CSVint As CSVinterface
    
    Set CSVint = New CSVinterface
    With CSVint.parseConfig
        .path = Environ("USERPROFILE") & "\Desktop\Chinese CSV.csv"
        .utf8EncodedFile = True                                         'The file is UTF-8 encoded
    End With
    With CSVint
        .ImportFromCSV .parseConfig
        .InsertField .fieldsBound + 1, "New field"                      'Insert a field named "New field"
    End With
    Set CSVint = Nothing
End Sub
```

[Back to Methods overview](https://ws-garcia.github.io/VBA-CSV-interface/api/methods/)