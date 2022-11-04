## excel 엑셀 파일 테스트 


```
       //given
        FileInputStream fileInputStream = new FileInputStream(filePath);
        MockMultipartFile mockMultipartFile = new MockMultipartFile(fileName, fileName + "." + contentType, contentType, fileInputStream);
//        HSSFWorkbook wb = new HSSFWorkbook(mockMultipartFile.getInputStream());
//        Workbook workbook = new XSSFWorkbook(new File(filePath));
        Workbook workbook = new HSSFWorkbook(mockMultipartFile.getInputStream());
        Sheet sheet = workbook.getSheetAt(0);

        for (Row row : sheet) {
            for (Cell cell : row) {
                switch (cell.getCellType()) {
                    case STRING:
                        System.out.print(cell.getRichStringCellValue().getString() + " Cell Type -> " + cell.getCellType() + "  ");
                        break;
                    case NUMERIC:
                        System.out.print(cell.getNumericCellValue() + " Cell Type -> " + cell.getCellType() + "  ");
                        break;
                    default:
                        System.out.print("test");
                }
            }
            System.out.println();
        }
```