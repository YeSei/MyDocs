# 【2】Elasticsearch倒排索引

## 2.1 简介
按照某个字段，反向生成包含的主键索引列表。这些字段叫做term。主键索引列表叫做posting list。
如果有多个term，会生成term dictionary 和 term index。

## 2.2 工作过程
比如查找age=18 的过程就是先从term index找到18在term dictionary的大概位置，然后再从term dictionary里精确地找到18这个term，然后得到一个posting list或者一个指向posting list位置的指针。

联合索引查询，就是将查找到的两个posting list进行与。