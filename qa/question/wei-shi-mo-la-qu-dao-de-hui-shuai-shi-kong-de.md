---
description: 国家外汇管理局
---

# 为什么拉取到的汇率是空的

### 问题描述

每日拉取到的花费以及收入的数据，需要针对汇率进行换算成人民币，在报表中显示。

因为每日的汇率都不同，所以需要在外汇管理局网站上拉取当天最新的汇率表。

偶现拉回来的数据中汇率为空。

### 问题排查

经排查，当日期为周末或者节假日时，不能拉取到当前日期的汇率，因为这种时间下汇率不更新，还是沿用上一个工作日的汇率。

### 问题解决

在请求api的参数中，延长起始时间的天数，比如时间跨度为15天，用来保证拉取到最近工作日的汇率数据。

### 链接示例

[https://www.safe.gov.cn/AppStructured/hlw/exportRMBExcel.do?startDate=2022-07-17\&endDate=2022-07-17\&queryYN=true](https://www.safe.gov.cn/AppStructured/hlw/exportRMBExcel.do?startDate=2022-07-17\&endDate=2022-07-17\&queryYN=true)
