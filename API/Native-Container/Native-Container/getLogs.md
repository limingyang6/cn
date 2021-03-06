# getLogs


## 描述
查询单个容器日志


## 请求方式
GET

## 请求地址
https://nativecontainer.jdcloud-api.com/v1/regions/{regionId}/containers/{containerId}:getLogs

|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**regionId**|String|True| |Region ID|
|**containerId**|String|True| |Container ID|

## 请求参数
|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**tailLines**|Integer|False| |返回日志文件中倒数 tailLines 行，如不指定，默认从容器启动时或 sinceSeconds 指定的时间读取。<br>|
|**sinceSeconds**|Integer|False| |返回相对于当前时间之前sinceSeconds之内的日志。<br>|
|**limitBytes**|Integer|False| |限制返回的日志文件内容字节数，取值范围 [1-4]KB，最大 4KB.<br>|


## 返回参数
|名称|类型|描述|
|---|---|---|
|**result**|Result| |
|**requestId**|String| |

### Result
|名称|类型|描述|
|---|---|---|
|**logs**|Object| |

## 返回码
|返回码|描述|
|---|---|
|**200**|OK|
|**400**|Invalid parameter|
|**401**|Authentication failed|
|**404**|Not found|
|**500**|Internal server error|
|**503**|Service unavailable|
