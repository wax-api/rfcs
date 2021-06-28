# Wax API DSL规范 (Wax API DSL Specification)
- 状态：Draft
- 作者：Zhang Ji zhangji@parsec.com.cn

## 1. 引言
什么是Wax API DSL？
> Wax API DSL是采用Wax Schema定义的、尽量与openapi兼容的、对人友好的规则，用来定义项目接口文档。

## 2. 全局定义
Wax API DSL的字段顺序是为了便于开发者理解而规定的固定顺序，所有字段顺序不能随意颠倒。
```json5
{
    "endpoints[]": "#EndpointDSL",
    "models": {"/.+/": "#ObjectDSL"},
    "schemaLambdas": {"/.+/": "string"},  //值是python源码，返回值不重要，目的是修改传入的schema变量
    "dataLambdas": {"/.+/": "string"},  //值是python源码，返回值不重要，目的是修改传入的data变量
    "mocks[]": "#MockDSL",
    "securitySchemes[]": {"/.+/": "#SecurityDSL"}
}
```

## 3. Endpoint DSL定义
```json5
{
  "method": "string",
  "path": "string",
  "summary": "string",
  "description": "string",
  "security[]": "string",
  "requestParam": {
    "path": "#ObjectDSL",
    "query": "#ObjectDSL",
    "header": "#ObjectDSL",
    "cookie": "#ObjectDSL"
  },
  "requestBody": {
    "schema": "#Schema",
    "contentType": "string"
  },
  "response": {
    "/[1-5]\\d\\d/": {
      "schema": "#Schema",
      "header": "#ObjectDSL",
      "contentType": "string",
      "lambdas[]": "string"  //以#开头的是dataLambdas引用，非#开头的是源码
    }
  },
  "lambdas[]": "string"  //以#开头的是schemaLambdas引用，非#开头的是源码
}
```

## 4. Object DSL定义
Object DSL只能是对象类型。
```json
{
    "/.+/": "#Schema"
}
```

## 5. Security DSL定义
```json5
[
    [
        {
            "type": ["string", {"enum": ["http"]}],
            "description": "string",
            "scheme": "string", //例如：basic, bearer等
        },
        {
            "type": ["string", {"enum": ["apiKey"]}],
            "description": "string",
            "name": "string",
            "in": "string",
        },
        {
            "type": ["string", {"enum": ["oauth2"]}],
            "description": "string",
            "flows": {
                "authorizationUrl": "string",
                "tokenUrl": "string",
                "refreshUrl": "string",
                "scopes": {"/.+/": "string"}  //scope_key: description
            },
        }
    ],
    {"logic": "anyOf"}
]
```

## 6. Mock DSL定义
```json5
{
    "method": "string",
    "path": "string",
    "key": "string",  //主键
    "summary": "string",  //名称
    "from": "string",  //克隆来源主键
    "statusCode": "integer",
    "contentType": "string",
    "mockjs": "object",  //以data为唯一key的json
    "data": "object",  //以data为唯一key的json，虚拟的属性，实际存数据库，也不能被导出(因为数据量可能过大)
    "header": "object"
}
```
