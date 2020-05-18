---
title: Davinci-二次开发系列06：导出excel合并单元格
date: 2020-05-14 12:00:00
categories: 
 - [Davinci]
 - [可视化工具]
tags:
 - BI
 - Davinci
---
# 前言

​	系统数据导出excel已经很具普遍性，不单单是BI有这需求，表单性质的数据大都希望直接导出excel，这个需求甚至比邮件接收更加突出。

​	Davinci导出excel有两种方式，都在dashboard页面

1. 单个widget导出
2. 整个dashboard导出，多个sheet页，一个sheet页对应一个widget数据
<!--more-->
# 表格数据

​	Davinci透视驱动和图表驱动下都有表格的功能，表格中维度指标是自动合并的，效果如下：

## 图表驱动

![table-chart](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-chart.jpg)

## 透视驱动

![table-pivot](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-pivot.jpg)

# 表格导出excel	

​	这里先不考虑图形效果的数据导出，表格数据更希望导出excel是相同的样式，也就是自动合并单元格，现有的表格数据导出效果如下：

## 图表驱动

![table-chart-old](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-chart-old.jpg)

## 透视驱动

![table-pivot-old](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/talble-pivot-old.jpg)

# 图表驱动表格excel自动合并单元格

## 合并后效果图

### 图表驱动

![table-chart-now](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-chart-now.jpg)

### 透视驱动

![table-pivot-now](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-pivot-now.jpg)

## 了解现有逻辑

```
<dependency>
	<groupId>org.apache.poi</groupId>
	<artifactId>poi</artifactId>
	<version>3.9</version>
</dependency>  
```

通过代码和依赖可以看出采用的poi处理导出excel

## 改造计划

​	如果通过现有的方式加入合并单元格的逻辑改造有点麻烦，这里我们直接找较完善的poi合并单元格的方法来直接引用并调用，如果有更好的方式可以一起讨论交流，欢迎留言。

​	参考poi合并单元格：https://www.cnblogs.com/mr-wuxiansheng/p/7930378.html

## 代码改造

​	参考上面链接这里直接提供改造后端的代码

### 一

新增PoiInfo实体类，严格意义上这应该是DO这样我们放的DTO下面

```
package edp.davinci.dto.poiDto;
import lombok.Data;
import java.io.Serializable;
/**
 * POI Excel报表导出，列合并实体<br>
 */
@Data
public class PoiInfo implements Serializable{
    private static final long serialVersionUID = 1L;
    private String content;
    private String oldContent;
    private int rowIndex;
    private int cellIndex;
    public PoiInfo() {
    }
    public PoiInfo(String content, String oldContent, int rowIndex,
                   int cellIndex) {
        this.content = content;
        this.oldContent = oldContent;
        this.rowIndex = rowIndex;
        this.cellIndex = cellIndex;
    }
    @Override
    public String toString() {
        return "PoiInfo [content=" + content + ", oldContent=" + oldContent
                + ", rowIndex=" + rowIndex + ", cellIndex=" + cellIndex + "]";
    }
}
```

### 二

废弃writeLine方法，新增createSheet方法

```
\server\src\main\java\edp\davinci\service\excel\AbstractSheetWriter.java
```

```
	/**
     * @param queryColumns 标题集合 queryColumns的长度应该与list中的model的属性个数一致
     * @param maps 内容集合
     * @param mergeIndex 合并单元格的列
     */
    public Boolean createSheet(Sheet sheet,List<QueryColumn> queryColumns, Map<String, List<Map<String, Object>>> maps, int[] mergeIndex){
        if (queryColumns.size()==0){
            return false;
        }
        int n = 0;
        for(Map.Entry<String, List<Map<String, Object>>> entry : maps.entrySet()){
            /*初始化head，填值标题行（第一行）*/
            Row row0 = sheet.createRow(0);
            for(int i = 0; i<queryColumns.size(); i++){
                /*创建单元格，指定类型*/
                Cell cell_1 = row0.createCell(i, Cell.CELL_TYPE_STRING);
                cell_1.setCellValue(queryColumns.get(i).getName());
            }
            /*得到当前sheet下的数据集合*/
            List<Map<String, Object>> list = entry.getValue();
            /*遍历该数据集合*/
            List<PoiInfo> poiInfos = new ArrayList();
            if(true){
                Iterator iterator = list.iterator();
                int index = 1;
                while (iterator.hasNext()){
                    Row row = sheet.createRow(index);
                    /*取得当前这行的map，该map中以key，value的形式存着这一行值*/
                    Map<String, String> map = (Map<String, String>)iterator.next();
                    /*循环列数，给当前行塞值*/
                    for(int i = 0; i<queryColumns.size(); i++){
                        String old = "";
                        /*old存的是上一行统一位置的单元的值，第一行是最上一行了，所以从第二行开始记*/
                        if(index > 1){
                            old = poiInfos.get(i)==null?"": poiInfos.get(i).getContent();
                        }
                        /*循环需要合并的列*/
                        for(int j = 0; j < mergeIndex.length; j++){
                            if(index == 1){
                                /*记录第一行的开始行和开始列*/
                                PoiInfo poiInfo = new PoiInfo();
                                poiInfo.setOldContent(String.valueOf(map.get(queryColumns.get(i).getName())));
                                poiInfo.setContent(String.valueOf(map.get(queryColumns.get(i).getName())));
                                poiInfo.setRowIndex(1);
                                poiInfo.setCellIndex(i);
                                poiInfos.add(poiInfo);
                                break;
                            }else if(i > 0 && mergeIndex[j] == i){
                                /*这边i>0也是因为第一列已经是最前一列了，只能从第二列开始*/
                                /*当前同一列的内容与上一行同一列不同时，把那以上的合并, 或者在当前元素一样的情况下，前一列的元素并不一样，这种情况也合并*/
                                /*如果不需要考虑当前行与上一行内容相同，但是它们的前一列内容不一样则不合并的情况，把下面条件中
　　　　　　　　　　　　　　　　　　||poiModels.get(i).getContent().equals(map.get(title[i])) && !poiModels.get(i - 1).getOldContent().equals(map.get(title[i-1]))去掉就行*/
                                if(!poiInfos.get(i).getContent().equals(String.valueOf(map.get(queryColumns.get(i).getName()))) || poiInfos.get(i).getContent().equals(String.valueOf(map.get(queryColumns.get(i).getName())))
                                        && !poiInfos.get(i - 1).getOldContent().equals(String.valueOf(map.get(queryColumns.get(i-1).getName())))){
                                    /*当前行的当前列与上一行的当前列的内容不一致时，则把当前行以上的合并*/
                                    CellRangeAddress cra=new CellRangeAddress(poiInfos.get(i).getRowIndex(), index - 1, poiInfos.get(i).getCellIndex(), poiInfos.get(i).getCellIndex());
                                    //在sheet里增加合并单元格
                                    sheet.addMergedRegion(cra);
                                    /*重新记录该列的内容为当前内容，行标记改为当前行标记，列标记则为当前列*/
                                    poiInfos.get(i).setContent(String.valueOf(map.get(queryColumns.get(i).getName())));
                                    poiInfos.get(i).setRowIndex(index);
                                    poiInfos.get(i).setCellIndex(i);
                                }
                            }
                            /*处理第一列的情况*/
                            if(mergeIndex[j] == i && i == 0 && !poiInfos.get(i).getContent().equals(String.valueOf(map.get(queryColumns.get(i).getName())))){
                                /*当前行的当前列与上一行的当前列的内容不一致时，则把当前行以上的合并*/
                                CellRangeAddress cra=new CellRangeAddress(poiInfos.get(i).getRowIndex(), index - 1, poiInfos.get(i).getCellIndex(), poiInfos.get(i).getCellIndex());
                                //在sheet里增加合并单元格
                                sheet.addMergedRegion(cra);
                                /*重新记录该列的内容为当前内容，行标记改为当前行标记*/
                                poiInfos.get(i).setContent(String.valueOf(map.get(queryColumns.get(i).getName())));
                                poiInfos.get(i).setRowIndex(index);
                                poiInfos.get(i).setCellIndex(i);
                            }

                            /*最后一行没有后续的行与之比较，所有当到最后一行时则直接合并对应列的相同内容*/
                            if(mergeIndex[j] == i && index == list.size()){
                                CellRangeAddress cra=new CellRangeAddress(poiInfos.get(i).getRowIndex(), index, poiInfos.get(i).getCellIndex(), poiInfos.get(i).getCellIndex());
                                //在sheet里增加合并单元格
                                sheet.addMergedRegion(cra);
                            }
                        }
                        Cell cell = row.createCell(i, Cell.CELL_TYPE_STRING);
                        cell.setCellValue(String.valueOf(map.get(queryColumns.get(i).getName())));
                        /*在每一个单元格处理完成后，把这个单元格内容设置为old内容*/
                        poiInfos.get(i).setOldContent(old);
                    }
                    index++;
                }
            }
            n++;
        }
        return true;
    }
```

### 三

放弃原来template.query 使用lambda表达式的调用方式（注释部分），修改如下

```
\server\src\main\java\edp\davinci\service\excel\SheetWorker.java
```

```
//            template.query(sql, rs -> {
//                Map<String, Object> dataMap = Maps.newHashMap();
//                for (int i = 1; i <= rs.getMetaData().getColumnCount(); i++) {
//                    dataMap.put(SqlUtils.getColumnLabel(queryFromsAndJoins, rs.getMetaData().getColumnLabel(i)), rs.getObject(rs.getMetaData().getColumnLabel(i)));
//                }
//                writeLine(context, dataMap);
//                count.incrementAndGet();
//            });

            List<Map<String, Object>> list = template.queryForList(sql);
            // 获取合并列
            List<Integer> listIt = new ArrayList<Integer>();
            for (int j = 0; j < context.getQueryColumns().size(); j++) {
                QueryColumn queryColumn = context.getQueryColumns().get(j);
                if(!queryColumn.getType().equals("value")){
                    listIt.add(j);
                }
            }
            int[] mergeIndex = new int[listIt.size()];
            int i = 0;
            for(Integer it: listIt){
                mergeIndex[i++] = it.intValue();
            }

            Map<String, List<Map<String, Object>>> map = new HashMap();
            map.put("合并单元格数据", list);
            createSheet(context.getSheet(),context.getQueryColumns(),map,mergeIndex);

```

完成以上配置，效果就有了


# 存在问题

## 透视驱动表头样式

​	目前无法做到透视驱动的表头样式，数据导出excel也仅仅和图表驱动类似的单元格数据的合并。

![table-pivot2](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-pivot2.jpg)


## 表头样式进行分组合并影响导出

​	表头样式进行分组合并设置，会影响导出，后续再进行处理。

![table-merge](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-merge.jpg)

## 图表驱动表格合并存在不合理情形

数据第一层维度数据不同，第二次数据相同合并，似乎不合理，excel做了处理，这样就不一致，如下情况：

![table-issue](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/06/table-issue.jpg)

# 交流学习

学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。