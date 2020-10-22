---
layout: default
title: Getting Started
parent: Home
nav_order: 1
description: "Introduction to the VBA CSV interface class."
---

# Getting Started
{: .fs-9 }

---

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

In order to be able to use `CSVinterface.cls` within your project, please review the [installation instructions](https://ws-garcia.github.io/VBA-CSV-interface/home/installation.html).

The CSV files are special kind of tabulated plain text data widely used in data exchange. There is no globally accepted standard format for that kind of files, however, out there are well formed standards such as [RFC-4180](https://www.ietf.org/rfc/rfc4180.txt) proposed by The Internet Society.
Although many solutions has been developed for work with CSV files into VBA, including projects from [@sdkn104](https://github.com/sdkn104/VBA-CSV) and [@Senipah](https://github.com/Senipah/VBA-Better-Array) on GitHub, the performance philosophy conduce me to the development of a VBA class module that allows users exchange data between VBA arrays and CSV files at superior speed for the VBA programing language.

## Advantages
* Fully compliant with RFC4180 CSV standard.
* Exported data is 100% Excel spreadsheet compatible.
* Writes and reads files at high speed.
* Minimal Memory overload.
* User have the option to import only certain range of records from given CSV file.
* Auto exclude any quote mark when data is imported.
* Simple code logic that allows you easy modify and enhance it!

## Philosophy
The VBA CSVinterface class module is designed for gain advantage from the well-structured CSV files, this means, there isn't automatic syntax check, given the user decide how the class will works. This can be seen as a weakness, but the class get a speed-up on writing and reading procedures at time the user controls how the file is interpreted, keeping in mind that, in fact, VBA is a language with slow code execution speed. Under this idealization the developed solution complies with the RFC4180 standard for user specified CSV document format.

## Usage
Import whole CSV file into an VBA array

```vb
Dim CSVix As CSVinterface
Dim MyArray() As String
Set CSVix = New CSVinterface
Call CSVix.OpenConnection(fileName)
Call CSVix.ImportFromCSV
Call CSVix(MyArray) 'Dumps the data to array
Set CSVix = Nothing
```

Import a range of records from a CSV file into a VBA array

```vb
Dim CSVix As CSVinterface
Dim MyArray() As String
Set CSVix = New CSVinterface
CSVix.StartingRecord = 10
CSVix.EndingRecord = 20
Call CSVix.OpenConnection(fileName)
Call CSVix.ImportFromCSV
Call CSVix(MyArray) 'Dumps the data to array
Set CSVix = Nothing
```

Set the char to escape special fields

```vb
CSVix.EscapeChar = EscapeType.NullChar
CSVix.EscapeChar = EscapeType.Apostrophe
CSVix.EscapeChar = EscapeType.DoubleQuotes
```

Set fields and records delimiters

```vb
CSVix.FieldsDelimiter = ";"
CSVix.RecordsDelimiter = vbCrLf
```

Define the data processing behavior

```vb
CSVix.QuotingMode = QuotationMode.Critical 'default
CSVix.QuotingMode = QuotationMode.All
```

Get the encoding of the last opened CSV file

```vb
Dim ENC as String
ENC = CSVix.FileEncoding
```

### Limitations
* __Data Format__: _Keep in mind that the class doesn't distinguish between number, dates and strings, all data is read as text and you can put in an Excel sheet to let Microsoft software format it._

## Benchmark
The benchmark provided here is focused on the supposed most critical operation, this is the parse one when working with CSV files. Although, benchmark for the exportation procedure is given on. 

The class was tested against two solutions (the one from [@Senipah](https://github.com/Senipah/VBA-Better-Array) and the other from [@sdkn104](https://github.com/sdkn104/VBA-CSV)) using a laptop running `Win 10 Pro x64, Intel® Core™ i7-4500U CPU @1.80-2.40 GHz, 8 GB RAM, Excel 2019 x86`. The test consists on a 100k calls to the import procedure over three (3) different files, each of this with three records (3) and four fields (4), for an overall work load of 1.2MM fields. The CSV files are:
* _RFC-4180_OH.csv_: **OH**- Only the teaders are quoted (4 fields)
* _RFC-4180_HF.csv_: **HF**- Half of fields are quoted (6 fields)
* _RFC-4180_AF.csv_: **AF**- All fields are quoted (12 fields) 
* *Demo_400000records.csv*: **LargeF**- 1.2MM fields.
* *Demo_Headed_400000records.csv*: **LargeFQ**- 1.2MM fields sorrounded by double quotes.

The main objective of this test is to measure the performance of the different procedures against the possible configurations of a potential CSV file. The test results can help answer the following questions: does the number of fields to be escaped affect the performance of the procedure? If yes, in what magnitude? The test also includes benchmark for parse to a CSV file of considerable size.

_NOTE: The table below shows the benchmark results, in seconds, for the currently tested procedures. Some projects was excluded from the benchmark due they does not complies with the RFC4180 standard_.

|*Procedure (Author)*|*OH|*HF*|*AF*|*LargeF*|*LargeFQ*|
|:--------------------------|-----------------:|----------------:|----------------:|----------------:|----------------:|
|*ImportFromCSV(W. García)*|**2.7031**|**2.7500**|**2.6719/0.9531**|**2.9844**|**4.5469/2.4844**|
|*FromCSV(@Senipah)*|N/A|N/A|N/A||N/A||N/A|
|*ParseCSVToArray/ADO (@sdkn104)*|3.5000|3.7969|4.5156|7.2812|11.7422|

In the above results, the 2nd value, for cells with two values, is obtained when setting `QuotingMode = QuotationMode.All`. As we can see, the class performance gets a little improve using this configuration. Keep in mind that not all CSV files can be successful imported using the previous tweaking.

The image bellow shows the overall performance for the CSV interface class.

![BenchMark](Benchmark.png)

## Licence
Copyright (C) 2020  [W. García](https://github.com/ws-garcia/VBA-CSV-interface/).

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program.  If not, see <https://www.gnu.org/licenses/>.