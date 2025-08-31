+++
date = '2025-08-31T10:34:54+08:00'
draft = false
title = 'Java双重循环优化技巧'
categories = ['Java']
tags =  ["Java", "小技巧"]
+++

## 1、两种写法比较

####  1.1 直接双重For循环

```java
public static void main(String[] args) {

    // 用于计算循环次数
    int count = 0;

    // 老公组
    List<Couple> husbands = new ArrayList<>();
    husbands.add(new Couple(1, "梁山伯"));
    husbands.add(new Couple(2, "牛郎"));
    husbands.add(new Couple(3, "干将"));
    husbands.add(new Couple(4, "工藤新一"));
    husbands.add(new Couple(5, "罗密欧"));

    // 老婆组
    List<Couple> wives = new ArrayList<>();
    wives.add(new Couple(1, "祝英台"));
    wives.add(new Couple(2, "织女"));
    wives.add(new Couple(3, "莫邪"));
    wives.add(new Couple(4, "毛利兰"));
    wives.add(new Couple(5, "朱丽叶"));

    for (Couple husband : husbands) {
        for (Couple wife : wives) {
            // 记录循环的次数
            count++;
            if (husband.getFamilyId().equals(wife.getFamilyId())) {
                System.out.println(husband.getUserName() + "爱" + wife.getUserName());
                // 牵手成功，换下一位男嘉宾
                break;
            }
        }
    }

    System.out.println("----------------------");
    System.out.println("循环了：" + count + "次");
}
```



#### 1.2 其中一个List转Map后再匹配

```java
public static void main(String[] args) {

    // 用于计算循环次数
    int count = 0;

    // 老公组
    List<Couple> husbands = new ArrayList<>();
    husbands.add(new Couple(1, "梁山伯"));
    husbands.add(new Couple(2, "牛郎"));
    husbands.add(new Couple(3, "干将"));
    husbands.add(new Couple(4, "工藤新一"));
    husbands.add(new Couple(5, "罗密欧"));

    // 老婆组
    List<Couple> wives = new ArrayList<>();
    wives.add(new Couple(1, "祝英台"));
    wives.add(new Couple(2, "织女"));
    wives.add(new Couple(3, "莫邪"));
    wives.add(new Couple(4, "毛利兰"));
    wives.add(new Couple(5, "朱丽叶"));

    // 给女嘉宾发牌子
    Map<Integer, Couple> wivesMap = new HashMap<>();
    for (Couple wife : wives) {
        // 女嘉宾现在不在List里了，而是去了wivesMap中，前面放了一块牌子：男嘉宾的号码
        wivesMap.put(wife.getFamilyId(), wife);
        count++;
    }

    // 男嘉宾上场
    for (Couple husband : husbands) {
        // 找到举着自己号码牌的女嘉宾
        Couple wife = wivesMap.get(husband.getFamilyId());
        System.out.println(husband.getUserName() + "爱" + wife.getUserName());
        count++;
    }

    System.out.println("----------------------");
    System.out.println("循环了：" + count + "次");
}
```



## 2、结论

#### 🔍 两种写法的区别

| 写法               | 查找方式                              | 时间复杂度    | 循环次数                                   | 原理                                                   |
| ------------------ | ------------------------------------- | ------------- | ------------------------------------------ | ------------------------------------------------------ |
| 第一种（`Map`）    | 通过 `HashMap` 的 `get(key)` 直接定位 | **O(1)** 平均 | `wives.size()` + `husbands.size()` ≈ 10 次 | 哈希表用哈希函数直接计算出存储位置，查找几乎是瞬时的   |
| 第二种（双层循环） | 遍历 `wives` 列表逐个比对             | **O(n²)**     | `husbands.size()` × `wives.size()` = 25 次 | 每个丈夫都要从头到尾扫描所有妻子，才能找到匹配的那一个 |

#### ⚡ 为什么第一种更快

1. **数据结构不同**
   - 第一种用 `HashMap` 存储妻子信息，`key` 是 `familyId`，查找时直接用哈希算法定位到内存位置，平均只需一次操作（O(1)）。
   - 第二种用 `List` 存储妻子信息，查找时只能从头到尾遍历（O(n)）。
2. **循环次数少**
   - 第一种：
     - 第一次循环：把妻子放进 `Map`（n 次）
     - 第二次循环：丈夫直接用 `get()` 找妻子（n 次）
     - 总共 **2n 次**
   - 第二种：
     - 双层循环：n × n 次
     - 总共 **n² 次**
   - 当 n 很大时，n² 会比 2n 大很多。
3. **时间复杂度差异**
   - 第一种：O(n)
   - 第二种：O(n²)
   - 当数据量从 5 增加到 10 万时，第一种依然很快，第二种会慢得不可忍受。

#### 📌 举个例子

假设有 10 万对夫妻：

- 第一种：大约执行 **20 万次** 操作
- 第二种：大约执行 **100 亿次** 操作 差距是 **五万倍** 以上，这就是为什么第一种更快。

✅ **总结**： 第一种快的原因是 **用哈希表（HashMap）将查找从 O(n) 降到了 O(1)**，从而把整体复杂度从 O(n²) 降到了 O(n)。

## 3、应用

#### 1️⃣ 电商订单匹配

**场景**：

- 有一个订单列表（包含商品ID），需要快速匹配商品库里的商品信息（价格、库存、促销信息）。

**用法对比**：

- **双层循环**：每个订单商品都要遍历整个商品库。
- **HashMap**：商品库先转成 `Map<商品ID, 商品对象>`，订单直接用 `get()` 查找。

**好处**：在大促活动（如双11）时，能显著减少 CPU 占用，提升响应速度。

#### 2️⃣ 学生与成绩匹配

**场景**：

- 有一个学生名单和一个成绩单，需要快速找到每个学生的成绩。

**用法对比**：

- **双层循环**：每个学生都要遍历成绩单。
- **HashMap**：成绩单先转成 `Map<学号, 成绩>`，直接查找。

**好处**：适合学校成绩管理系统、在线考试系统等。

#### 3️⃣ 物联网设备状态查询

**场景**：

- 有几万台传感器设备，每台设备有唯一的 `设备ID`，需要根据实时上报的 `设备ID` 找到设备信息（位置、类型、阈值等）。

**用法对比**：

- **双层循环**：每条数据都要遍历设备列表。
- **HashMap**：设备列表先转成 `Map<设备ID, 设备对象>`，直接查找。

**好处**：在秒级数据上报的场景下，能保证系统实时响应。
