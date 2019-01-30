---
layout: post
title:  JSON 生成 EXECL
date:   2017-07-24 00:00:00 +0800
categories: Python
tag: 小工具
---

* content
{:toc}


# 使用PyThon 制作 小工具【JSON存入Execl】

### 1.数据准备

新建一个后缀为.json的文件，内容如下：

```
[
	{"id":"2","title":"a","url":"www.layui.com"},
	{"id":"1","title":"b","title2":"title2","url":"www.bejson.com"},
	{"id":"3","title":"c","title3":"title2"}
]
```

### 2.写代码

```
import xlwt
import json
import logging

#日志配置
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
my_logger = logging.getLogger(__name__)


def read_json_file():
	#json文件的路径
    stream = open('C:\\Users\\samsung\\Desktop\\heh.json', encoding='UTF-8')
    jsobj = json.load(stream)
    return jsobj


def json_to_excel():
    json_file = read_json_file()
    print(json_file)

    workbook = xlwt.Workbook()
    sheet1 = workbook.add_sheet('student')
    all_column_list = json_all_column(json_file)
    for x in range(0, len(all_column_list)):
        sheet1.write(0, x, all_column_list[x])

    for i in range(0, len(json_file)):
        keys_list = list(json_file[i].keys())
        values_list = list(json_file[i].values())
        for j in range(0, len(keys_list)):
            if keys_list[j] in all_column_list:
                values_j = values_list[j]
                keys_j = all_column_list.index(keys_list[j])
                my_logger.info("需要把值%s:放入表格的第%s行", str(values_j), str(keys_j))
                # 行 列 值
                sheet1.write(i+1, all_column_list.index(keys_list[j]), values_j)
            else:
                print(all_column_list[j]+"不存在")
                pass
    workbook.save('student3.xls')


'''
        现在需要找出 所有的字段
        所有行 的keys 的并集
'''


def json_all_column(json_file):
    merge_list = []

    for i in range(0, len(json_file)):
        ll = list(json_file[i].keys())
        # print("ll:" + str(ll))
        for j in range(0, len(ll)):
            if ll[j] not in merge_list:
                # print("ll[j]:" + str(ll[j]))
                merge_list.append(ll[j])
        print("继续 下一条 数据")
    print("merge_list:" + str(merge_list))
    my_logger.info("行数：%s", str(len(json_file)))
    my_logger.info("列数：%s", str(len(merge_list)))
    return merge_list


if __name__ == '__main__':
    json_to_excel()

```

### 3.结果

![[![1548859696727.png](https://i.loli.net/2019/01/30/5c51b965d419f.png)](https://i.loli.net/2019/01/30/5c51b965d419f.png)]()

