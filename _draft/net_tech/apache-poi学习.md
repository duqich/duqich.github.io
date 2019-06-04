- API:http://www.cnblogs.com/LiZhiW/p/4313789.html
- 包路径：com.zl.base.core\com.zl.base.core.jxl\com.zl.base.core.taglib.datagrid\com.zl.base.core.taglib.datagridreport

## POI操作
- 创建输出流，新建Excel实例(HSSFWorkBook)
- 新建各种组件
- 用创建的实例写入输出流如excel.write(fos);
- 关闭输出流
## 基本组件
- HSSFWorkBook文档实例
- Sheet工作表
- Row行
- Cell单元格
- setCellValue()设置值
- setCellComment()设置批注
- cell.setCellStyle(style)设置样式
```java
HSSFWorkBook excel = new HSSFWorkBook();//创建实例
excel.createSheet("sheet1");.createRow(0).createCell(0).setCellValue(123);
```
## 调整文档属性

```java
excel.createInformationProperties();
DocumentSummaryInformation dsi = excel.getDocumentSummaryInformation();//对应属性里的来源
dsi.setCategory("没类别");
dsi.setCompany("没公司");
SummaryInformation si = excel.getSummaryInformation();//对应属性里的说明
si.setSubject("没主题");
si.setTitle("什么标题");
si.setAuthor("dqc");
```

## 创建批注
```java
HSSFWorkbook excel = new HSSFWorkbook();
HSSFSheet sheet = excel.createSheet("sheet1");
HSSFPatriarch patriarch = sheet.createDrawingPatriarch();
HSSFClientAnchor anchor = patriarch.createAnchor(0,0,0,0,5,1,8,3);
HSSFComment comment = patriarch.createCellComment(anchor);
comment.setString(new HSSFRichTextString("这是批注"));

sheet.createRow(2).createCell(2).setCellComment(comment);
```

## 创建单元格格式
- 从excel实例创建一个Style样式，将需要样式的cell赋予这个样式cell.setCellStyle(style);
```java
HSSFCellStyle style = excel.createCellStyle();
style.setDataFormat(HSSFDataFormat.getBuiltinFormat("m/d/yy h:mm"));

HSSFCell cell = sheet.createRow(0).createCell(0);
cell.setCellValue(new Date());
cell.setCellStyle(style);
```
## 合并单元格
- 创建CellRangeAddress实例，赋给对应的sheet实例
```java
CellRangeAddress range1 = new CellRangeAddress(0,1,0,6);
sheet.addMergedRegion(range1);
```

## 合并单元格的格式
- 其实就是选中单元格的其中一个，然后赋予style
```java
CellRangeAddress range1 = new CellRangeAddress(0,1,0,6);
sheet.addMergedRegion(range1);
HSSFCell cell = sheet.createRow(0).createCell(0);
cell.setCellValue(123);
HSSFCellStyle style = excel.createCellStyle();
style.setAlignment(HorizontalAlignment.CENTER);
cell.setCellStyle(style);
```

## 单元格颜色
```java
XSSFCellStyle style = excel.createCellStyle();
style.setFillForegroundColor(new XSSFColor(Color.RED));
style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
```
## 单元格大小
```java
sheet.setColumnWidth(1, 31 * 256);//设置第一列的宽度是31个字符宽度
row.setHeightInPoints(50);//设置行的高度是50个点
```

## 遍历sheet
```java
for (Row row : sheet)
{
    for (Cell cell : row)
    {
        System.out.print(cell + "\t");
    }
    System.out.println();
}
```

## 冻结行列
```java
sheet.createFreezePane(2, 3, 15, 25);//冻结行列
```

## 移动行列

```java
sheet.shiftRows(2, 4, 2);//把第3行到第4行向下移动两行
```

