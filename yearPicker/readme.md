# 日期选择器(深浅配色)
###  使用
```
<YearPicker v-model="year" @input="handlerInput" :year-disable="'no'"/>
```
###  说明

####  配色

默认浅色，深色在外层加 .dark

####  @input
选中年份事件
####  :year-disable
接受一个string
+ after:今年以后的年份被禁用
+ no:无禁用（默认）
+ before:今年以前的年份被禁用

###  有问题联系wjune212（wx）
