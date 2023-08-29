public static void main( String [] args ) {
    try {

        InputStream input = POIExample.class.getResourceAsStream( "qa.xls" );
        POIFSFileSystem fs = new POIFSFileSystem( input );
        HSSFWorkbook wb = new HSSFWorkbook(fs);


        for (int i = 0; i < wb.getNumberOfSheets(); i++) {
            HSSFSheet sheet = wb.getSheetAt(i);

            // Do your stuff        
        }

    } catch ( IOException ex ) {
        ex.printStackTrace();
    }
}
Iterator<Sheet> sheetIterator = workbook.iterator();
while (sheetIterator.hasNext()) {
    Sheet sheet = sheetIterator.next();
}

HSSF: POI Project's pure Java implementation of the Excel '97(-2007) file format.

HSSFSheet sheet = (HSSFSheet) sheetIterator.next();
XSSF: POI Project's pure Java implementation of the Excel 2007 OOXML (.xlsx) file format.

XSSFSheet sheet = (XSSFSheet) sheetIterator.next();
try {
        FileInputStream file = new FileInputStream(new File("Turto sarašas 2016.09.30.xlsx"));

        //Create Workbook instance holding reference to .xlsx file
        XSSFWorkbook workbook = new XSSFWorkbook(file);

        //Get first/desired sheet from the workbook
        XSSFSheet sheet = workbook.getSheetAt(0);

        //Iterate through each rows one by one
        Iterator<Row> rowIterator = sheet.iterator();

        while (rowIterator.hasNext()) {
            Row row = rowIterator.next();
            //For each row, iterate through all the columns
            Iterator<Cell> cellIterator = row.cellIterator();

            while (cellIterator.hasNext()) {

                Cell cell = cellIterator.next();
                //Check the cell type and format accordingly
                final DataFormatter df = new DataFormatter();
                String valueAsString = df.formatCellValue(cell);
                if (valueAsString.equals(barcode)) {
                    System.out.print("Hello" + row.getCell(0));
                    System.out.print("Hello" + row.getCell(3));
                } else if (!valueAsString.equals(barcode)) {
                    System.out.println(" Match not found");
                }
            }
        }

        file.close();
    } catch (IOException e) {
    }
javaexcelapache
package com.chegus.controller;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;


import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.CellType;
import org.apache.poi.ss.usermodel.Row;
import org.apache.poi.ss.usermodel.Sheet;
import org.apache.poi.ss.usermodel.Workbook;
import org.apache.poi.xssf.usermodel.XSSFCell;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import com.chegus.entity.Customers;

public class ExcelControler {

    private static final String STRING = null;
    private static final Number NUMBER = 0;

    String filepath = "C:\\Users\\divakar\\OneDrive\\Desktop\\CustomersData.xlsx";

    List<Customers> list = new ArrayList<>();
	private XSSFSheet sheet;

    public void saveCustomersData() throws IOException {
        long startTime = System.currentTimeMillis(); // Record start time

        FileInputStream inputStream = new FileInputStream(filepath);
        Workbook workbook = new XSSFWorkbook(inputStream);
        int c=0;
        int x=0;
        Iterator<Sheet> sheetIterator = workbook.iterator();
        while(sheetIterator.hasNext()) {
          XSSFSheet sheet = (XSSFSheet) sheetIterator.next();
		 int lastRowNum = sheet.getLastRowNum();
		 System.out.println(lastRowNum);
		 Iterator<Row> rowIterator = sheet.iterator();
		 if (rowIterator.hasNext()) {
		     rowIterator.next();
		 }
         System.out.println(c++ +"forloop");
      while (rowIterator.hasNext()) {
		XSSFRow row = (XSSFRow) rowIterator.next();

		Customers customer = new Customers();

		Map<String, Integer> columnIndexMap = getColumnIndexMap(sheet.getRow(0));

		Map<String, String> cellValueMap = columnIndexMap.entrySet().stream()
		        .collect(Collectors.toMap(Map.Entry::getKey, entry -> getCellValue(row, columnIndexMap, entry.getKey())));

		customer.setFirstName(cellValueMap.get("FirstName"));
		customer.setMidleName(cellValueMap.get("MidleName"));
		customer.setLastName(cellValueMap.get("LastName"));
		customer.setEmail(cellValueMap.get("Email"));
		customer.setPhone(cellValueMap.get("PhoneNo"));
		customer.setCity(cellValueMap.get("City"));

		list.add(customer);
		System.out.println(x++ +"whileloop");
      }

      for (Customers customer : list) {
		System.out.println(customer);
      }

      long endTime = System.currentTimeMillis();
      long elapsedTime = endTime - startTime;
      System.out.println("Time taken: " + elapsedTime + " milliseconds");
    }
    }
    private Map<String, Integer> getColumnIndexMap(XSSFRow headerRow) {
        Map<String, Integer> columnIndexMap = new HashMap<>();
        Iterator<Cell> cellIterator = headerRow.cellIterator();
        int columnIndex = 0;
        while (cellIterator.hasNext()) {
            XSSFCell headerCell = (XSSFCell) cellIterator.next();
            columnIndexMap.put(headerCell.getStringCellValue(), columnIndex++);
        }
        return columnIndexMap;
    }

    private String getCellValue(XSSFRow row, Map<String, Integer> columnIndexMap, String columnName) {
        Integer columnIndex = columnIndexMap.get(columnName);
        if (columnIndex != null) {
            XSSFCell cell = row.getCell(columnIndex);
            if (cell != null) {
                cell.setCellType(CellType.STRING);
                return cell.getStringCellValue();
            }
        }
        return null;
    }
}
