# C&CPP 学习笔记

## VS_Code 快捷键

- Ctrl + B 开关侧边栏
- Ctrl + D 选定当前字符串，多选后可同时更改
- Ctrl + J 合并多行
- Ctrl + G 导航到特定行
- Ctrl + T 转到工作区内的特定符号
- Ctrl + X 删除一行
- Ctrl + Backspace 删除上一个字符串
- Ctrl + Shift + D 重复行
- Ctrl + Shift + O 转到当前文件中的特定符号
- Ctrl + Shift + [ 代码折叠
- Ctrl + Shift + ] 代码展开
- Ctrl + Shift + Right/Left 选择整个词
- Shift + Alt + Up/Down 复制上下行
- Shift + Alt 列/框选择
- Alt + Up/Down 将当前行与上/下行互换

## CPP

### stirng 类型中以旧字符串初始化

```
string new_s= old_s.substr(pos,n);
```

从旧字符串的 **pos** 位置开始，数 **n** 个字符

### string 类型中字符替换

```
string new_s=old_s.replace(pos,len,use_s);
```

从旧字符串的 **pos** 位置开始，后续 **n** 个依次替换为 **use_s**

### lambda 函数

```
[capture](parameter_list) -> return_type {
    ...
}

auto fun=[capture](parameter_list) -> return_type{
    ...
}
```

在 `<sp1>` 中填入一个变量，仅起到引用值效果，在变量前加上 `&` 可起到引用传递某一个变量，若仅填入 `&` 代表对于所有局部变量都起到引用传递作用

使得函数定义简易化、便捷化

若想要起到调用自身的效果，可以在参数列表中填入自身 `auto &&fun`

### to_string

```
int num=1000;
string str=to_string(num); //str="1000"
```

快速将数字转化为字符串形式表示

### 迭代器与逆向迭代器

```
c.begin()=c.rend()+1
c.rbegin()=c.end()-1
```

等式右边的迭代器在无操作的情况下，不指向 C 的元素

### 遍历哈希表

```
unordered_map<int,int> umap;

for(auto a:umap){
    cout << a.first << a.second;    //pair<int,int>形式值传递
}

for(auto& a:umap){
    cout << a.first << a.second;    //pair<int,int>形式引用传递
}

for(auto it=umap.begin();it!=umap.end();it++){
    cout << it->first << it->second;//使用迭代器遍历
}
```

### 哈希表的 erase()方法

1. 如果删除的元素不存在，erase()方法不会进行操作

2. 若有多个相同目标键存在，仅删除第一个键值对

3. 会导致迭代器失效，需要返回一个新迭代器

### accumulate 累加求和

```
int sum = accumulate(vec.begin(), vec.end(), first) //first为初始值
```

### 异或计算

```
同0异1

1 ^ 0 = 1

0 ^ 1 = 1

1 ^ 1 = 0

x ^ x = 0

```
