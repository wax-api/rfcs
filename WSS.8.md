# Wax Schema规范 (Wax Schema Specification)
- 状态：Draft
- 作者：Zhang Ji zhangji@parsec.com.cn

## 1. 引言
什么是wax Schema?
> wax SchemaL是跟JSON Schema等价的、对人友好的规则，用来定义JSON元数据。

假设有一个web api，接受一个json请求，返回某个用户在某个城市关系最近的若干个好友。一个请求的例子如下：
```json
{
    "city" : "chicago", 
    "number": 20, 
    "user" : {
        "name": "Alex", 
        "age": 20
    }
}
```

对应的Wax Schema如下:
```json
{
    "city": "string",
    "number": "integer",
    "user": {
        "name": "string",
        "age": "integer"
    }
}
```

## 2. 类型关键字
类型关键词可分为这几类：
   * 简单类型："string", "number", "integer", "boolean", "null"。
   * 复合类型："object", "array"。在Wax Schema中一般不会显式使用集合类型关键词，因为有具体语法可以代替，除非只是希望笼统地表示类型。

## 2.1. 限定信息
如下这个Wax Schema可以定义一个人：
```json
{
    "name": "string",
    "age": "integer"
}
```
但如果要定义一个成年人，就需要对年龄字段做限定，变成：
```json
{
    "name": "string",
    "age": ["integer", {"minimum": 18}]
}
```
所以用来定义类型的Schema，既可以只是类型关键字，也可以是形如`[类型关键字，限定信息]`的数组。

## 3. 简单类型
### 3.1. 字符串
string支持的类型特定参数为：
   * 字符串长度，例如：`["string", {"minLength": 2, "maxLength": 4}]`
   * 正则表达式，例如：`["string", {"pattern" : "^(\\([0-9]{3}\\))?[0-9]{3}-[0-9]{4}$"}]`
   * 字符串Format，例如：`["string", {"format" : "date"}]`

### 3.2. 数值
数值类型包括"number"和"integer"。number可以是整数或浮点数，integer则只能是整数。"number"和"integer"的类型特定参数相同，可以限制倍数、范围：
   * 数值满足倍数，例如：`["number", {"multipleOf" : 10}]`
   * 数值范围，关键词：minimum, maximum, exclusiveMinimum, exclusiveMaximum (最大值、最小值、开区间最大值、开区间最小值)

### 3.3. 其他
布尔类型和null类型没有额外的类型特定参数。

## 4. 复合类型-数组
### 4.1. 数组成员类型
下面的Schema要求数组内所有元素都是数值：
```json
["number", {"array": true}]
```

下面的Schema表示整数的数组的数组：
```json
[
    [
        "integer",
        {"array": true}
    ],
    {"array": true}
]
```

如果Schema类型是一个数组，则要求数组内每个item按位置一一匹配：
```json
[
    ["number", "string"],
    {"array": true}
]
```

### 4.2. 数组是否允许额外成员
当Schema类型是数组时才生效，当additionalItems为true时，允许额外元素:
```json
[
    ["number", "string"],
    {"array": true, "additionalItems" : true}
]
```

### 4.3. 数组元素个数
关键字：minItems, maxItems。可以限制数组内元素的个数。
```json
["number", {"array": true, "minItems": 5, "maxItems": 10}]
```

### 4.4. 数组内元素是否必须唯一
关键字：uniqueItems。规定数组内的元素是否必须唯一。
```json
["number", {"array": true, "uniqueItems" : true}]
```

## 5. 复合类型-对象
### 5.1. 成员的Schema
```json
{
    "name": "string",
    "age": "integer",
    "address": {
        "city": "string",
        "country": "USA"
    }
}
```
对于上例中的Schema，合法的data是:
```json
{
    "name": "Froid",
    "age" : 26,
    "address" : {
        "city" : "New York",
        "country" : "USA"
    }
}
```

### 5.2. 批量定义成员Schema
如果key的收尾都是`/`，则表示key通过正则表达式匹配属性名。
```json
{
    "/^S_/": "string",
    "/^I_/": "integer"
}
```
可以匹配如下数据：
* `{"S_1" : "abc", "I_3" : 1}`

### 5.3. 必须出现的成员
关键字：required。规定哪些对象成员是必须的。
```json
{
    "name": "string",
    "age": ["integer", {"required": true}]
}
```

### 5.4. 成员依赖关系
关键字：dependencies。规定某些成员的依赖成员，不能在依赖成员缺席的情况下单独出现，属于数据完整性方面的约束。
```json
{
    "credit_card": ["string", {"dependencies": ["billing_address"]}]
}
```

### 5.5. 是否允许额外属性
关键字：additionaProperties。规定object类型是否允许出现不在properties中规定的属性，只能取true/false。
```json
[
    {"name": "string", "age": "integer"},
    {"additionalProperties": true}
]
```
### 5.6. 属性个数的限制
关键字：minProperties, maxProperties。规定最少、最多有几个属性成员。
```json
["object", {"minProperties": 2, "maxProperties": 3}]
```

## 6. 逻辑组合
关键字：allOf, anyOf, oneOf, not。从关键字名字可以看出其含义，满足所有、满足任意、满足一个。前三个关键字的使用形式是一致的，以anyOf为例说明其形式。

### 6.1. anyOf
```json
[
    ["string", "object"],
    {"logic": "anyOf"}
]
```

### 6.2. not
它告诉Json不能满足not所对应的Schema。
```json
[
    "string",
    {"logic": "not"}
]
```

## 7. 复杂结构
### 7.1. 引用
Wax Schema的引用支持智能查找，而不需要像JSON Schema那样使用精确的绝对路径。
```json
{
    "billing_address": "#Address",
    "shipping_address": "#Address"
}
```

## 8. 通用关键字
### 8.1. enum
表示枚举类型，enum的值可以是数组，也可以是字典，例如：
```json
[
    "string",
    {"enum": ["red", "green", "blue"]}
]
```
或者
```json
[
    "integer",
    {"enum": {"0": "禁用", "1": "启用"}}
]
```

### 8.2. metadata
关键字：title，default，example。只作为描述作用，不影响对数据的校验，与JSON Schema定义的关键字对应。

### 8.3. description
description作为最常用的字段，不属于metadata，而是可以出现在Schema[1]的位置上，例如：
```json
["string", "姓名"]
```

### 8.4. mock
可以使用mock关键词设置如何生成测试数据，类型不限，转化为JSON Schema时，会变成X-mock这个保留字段。例如：
```json
["string", "姓名", {"mock": "@cname"}]
```

## 9. 总结
下面的Schema不严格地、递归地定义了`#Schema`这个引用：
```json5
[
    [
        {
            "/.*/": "#Schema"  //Schema类型为对象时，所有value都应该是Schema
        },
        [
            "string",  //Schema类型为字符串时，表示简单类型
            {"enum": ["string", "number", "integer", "boolean", "null"]}
        ],
        [
            [
                "#Schema",
                "string"  //Schema[1]为字段描述
            ],
            {"array": true}  //Schema类型为数组 && Schema[1]类型为字符串时
        ],
        [
            [
                [
                    [
                        "#Schema",
                        ["#Schema", {"array": true}]
                    ],
                    {"logic": "anyOf"}
                ],
                "object"  //Schema[1]为限定信息
            ],
            {"array": true}  //Schema类型为数组 && Schema[1]类型为对象时
        ],
        [
            [
                [
                    [
                        "#Schema",
                        ["#Schema", {"array": true}]
                    ],
                    {"logic": "anyOf"}
                ],
                "string"  //Schema[1]为字段描述
                "object"  //Schema[2]为限定信息
            ],
            {"array": true}  //Schema类型为数组 && Schema[1]类型为字符串 && Schema[2]类型为对象时
        ]
    ],
    {"logic": "anyOf"}
]
```

## 10. 简洁形式
1. 允许并推荐将`{"key": [schema, {"array": true}]}`简化为`{"key[]": schema}`
2. 允许并推荐将`{"key": [schema, {"required": true}]}`简化为`{"key!": schema}`
3. 允许并推荐将`{"key": [schema, {"array": true, "required": true}]}`简化为`{"key[]!": schema}`
