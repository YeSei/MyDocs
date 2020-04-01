# 【2】Java深拷贝
1. Collections.copy方法
```
方法一：

ArrayList<Integer> mycopy=new ArrayList<Integer>();

mycopy=(ArrayList<Integer>) vec.clone();

方法二：

ArrayList<Integer> mycopy=new ArrayList<Integer>(Arrays.asList(new Integer[vec.size()]));
Collections.copy(mycopy, vec);
```

2. 序列化方法
3. 复写clone方法