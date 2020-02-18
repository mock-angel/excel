# Excel

Excel is a flutter and dart library for creating and updating excel-sheets for XLSX files.

## Usage

### In Flutter App

    import 'dart:io';
    import 'package:path/path.dart';
    import 'package:excel/excel.dart';

    ...
    
    var decoder = Excel.createExcel();

    /**
     * Create new Excel Sheet
     * var decoder = Excel.createExcel();
     *  
     * ------------ Or ------------
     * For Editing Pre-Existing Excel File
     * 
     * var file = "Path_to_pre_existing_Excel_File/NewExcel.xlsx";
     * var bytes = File(file).readAsBytesSync();
     * var decoder = Excel.decodeBytes(bytes, update: true);
     **/
     
    for (var table in decoder.tables.keys) {
      print(table);
      print(decoder.tables[table].maxCols);
      print(decoder.tables[table].maxRows);
      for (var row in decoder.tables[table].rows) {
        print("$row");
      }
    }

    /**
     * Define Your own sheet name
     * var sheet = 'SheetName'
     * 
     * ---------- Or ----------
     * Find the desired sheet by iterating throught the [existing sheets]:
     * var sheet;
     * for (var tableName in decoder.tables.keys) {
     *    if( desiredSheetName.toString() == tableName.toString() ){
     *      sheet = tableName.toString();
     *      break;
     *    }
     * }
     */

    // if [MySheetName] does not exist then it will be automatically created.
    var sheet = 'MySheetName';

      decoder
    ..updateCell(sheet, CellIndex.indexByString("A1"), "A1",
        fontColorHex: "#1AFF1A", verticalAlign: VerticalAlign.Top)
    ..updateCell(
        sheet, CellIndex.indexByColumnRow(columnIndex: 2, rowIndex: 0), "C1",
        wrap: TextWrapping.WrapText)
    ..updateCell(sheet, CellIndex.indexByString("A2"), "A2",
        backgroundColorHex: "#1AFF1A")
    ..updateCell(sheet, CellIndex.indexByString("E5"), "E5",
        horizontalAlign: HorizontalAlign.Right);
    
    // Save the file

    decoder.encode().then((onValue) {
      File(join("/Users/kawal/Desktop/excel.xlsx"))
        ..createSync(recursive: true)
        ..writeAsBytesSync(onValue);
    });
    
    ...

## Features coming in next version
On-going implementation for future:
- spanned rows (Comming Soon in future updates)
- spanned columns (Comming Soon in future updates)

## Important:
For XLSX format, this implementation only supports native Excel format for date, time and boolean type conversion.
In other words, custom format for date, time, boolean aren't supported and also the files exported from LibreOffice as well.