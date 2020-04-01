# 【3】Arrays.asList陷阱
> 使用`Arrays.asList`初始化的集合，实现RandomAccess和Serializable接口。但是为只读的。任何对此集合的修改操作都是会报`UnsupportedOperationException`异常的。
```
List<Integer> list = Arrays.asList(1,2,3);
```
> 正确的写法为：
```
List<Integer> list = new ArrayList<>(Arrays.asList(1,2,3));
```