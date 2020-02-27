---
title: Metabase-BI系列09：透视表PR功能实现解析
date: 2020-02-27 22:40:00
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---

​	上一篇文章介绍了Metabase透视表的开源PR，时间较早，基于Metabase V0.30版本的实现，而Metabase V0.33版本之后变化较大，所以如果在新版上实现PR中Metabase 复杂表头表格功能需要做改造。当然如果实力允许，你也可以按照自己的思路去实现。

<!--more-->
## 效果呈现

![talbedt](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/table_dt.gif)

![001](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/table_summary01.png)	

**![002](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/table_summary.png)**

## 实现原理

### 数据结构

Metabase返回的数据结构，通过前端进行封装，版本之间会存在差别

```
frontend\src\metabase\meta\types\Dataset.js
```

```
export type DatasetData = {
  cols: Column[],//列信息
  columns: ColumnName[],//备注：新版本没有columns字段
  rows: Row[],//行信息
  rows_truncated?: number,
  requested_timezone?: string,
  results_timezone?: string,
};
```

![summary01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/summary01.jpg)

### 新增的图表类型

由于Metabase组件化的代码，所以作者新增图表类型，不影响原来的功能和代码

```
frontend\src\metabase\visualizations\index.js
```

```
import SummaryTable from "./visualizations/SummaryTable";

registerVisualization(SummaryTable);
```

![summary03](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/summary03.jpg)



### 复杂表头图表设置项

```
const emptyStateSerialized: SummaryTableSettings = {
  groupsSources: [],
  columnsSource: [],
  valuesSources: [],
  unusedColumns: [],
  columnNameToMetadata: {},
};
```

- 行显示字段：多个分组嵌套
- 列显示字段：列字段只能有一个不支持多个进行嵌套
- 聚合字段（度量）
- 不可见字段：分组列表里面有，但是手动设置不显示的字段

行和列字段支持功能：

- 汇总
- 排序

![summary02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/summary02.jpg)

### 核心处理

在原有的table基础上进行的修改，主要添加了这个js实现计算汇总及后续处理

```
frontend\src\metabase\visualizations\lib\summary_table_datasetdata_builder.js
```

```
export const buildDatasetData = (//格式化数据集
  settings: SummaryTableSettings,
  mainResults: DatasetData,
  resultsProvider: ResultProvider,
): SummaryTableDatasetData => {
  const { columnsHeaders, cols, dimensions } = buildColumnHeaders(//生成列标题
    settings,
    mainResults,
  );
  const compressedColumnsHeaders = tryCompressColumnsHeaders(//压缩列标题处理
    settings,
    columnsHeaders,
  );
  const rowAssembler = getRowAssembler(columnsHeaders);//行数据装配

  const queryPlan = getQueryPlan(//生成合计
    settings,
    canTotalizeBuilder(mainResults.cols),
  );
  const rows = combineData(rowAssembler, queryPlan, settings, resultsProvider);//合并数据
  //生成列索引
  const columnIndexToFirstInGroupIndexes = buildColumnIndexToFirstInGroupIndexes(
    rows,
    settings,
  );
  //生成行索引
  const rowIndexesToColSpans = buildRowIndexesToColSpans(settings, rows, cols);

  return {
    columnsHeaders: compressedColumnsHeaders,
    cols,
    columns: cols.map(p => p.name),
    rows,
    columnIndexToFirstInGroupIndexes,
    isGrouped: columnIndex => columnIndex in columnIndexToFirstInGroupIndexes,
    rowIndexesToColSpans,
    dimensions,
  };
};
```

### js组件（lodash）数据处理

Lodash是一个著名的javascript原生库，不需要引入其他第三方依赖，像Jquery的一样，十分简洁，正是利用lodash实现数据的js处理。

```
    "lodash.flatmap": "^4.5.0",
    "lodash.get": "^4.1.2",
    "lodash.invert": "^4.3.0",
    "lodash.isequal": "^4.5.0",
    "lodash.mapkeys": "^4.6.0",
    "lodash.mapvalues": "^4.6.0",
    "lodash.orderby": "^4.6.0",
    "lodash.partition": "^4.6.0",
    "lodash.range": "^3.2.0",
    "lodash.set": "^4.1.2",
    "lodash.sortby": "^4.6.0",
    "lodash.unset": "^4.1.2",
    "lodash.values": "^4.3.0",
    "lodash.zip": "^4.2.0"
```



