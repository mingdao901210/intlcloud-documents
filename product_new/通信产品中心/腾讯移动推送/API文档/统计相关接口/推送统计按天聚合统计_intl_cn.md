##  接口说明
**请求方式**：POST。
**调用频率限制**：200次/小时。
```shell
https://api.tpns.tencent.com/v3/statistics/get_push_stat_overview
```
**接口功能**： 查询某个时间段内每天的推送转化汇总数据。

## 参数说明
#### 请求参数

| 参数名称  | 必选 | 类型   | 描述                                           |
| --------- | ---- | ------ | ---------------------------------------------- |
| startDate | 是   | string | startDate 查询限制：只能查询最近3个月内的数据 |
| endDate   | 是   | string | 数据为时间戳                                   |

#### 应答参数

| 参数名称             | 类型      | 描述                                                         |
| -------------------- | --------- | ------------------------------------------------------------ |
| retCode              | int       | 返回状态码                                                   |
| errMsg               | string    | 错误信息                                                     |
| PushStatOverviewData | JsonArray | 返回结果 [PushStatOverviewData]，PushStatOverviewData 结构变量见下表 |

#### PushStatOverviewData（Android）

| 参数名称     | 类型   | 说明     |
| ------------ | ------ | -------- |
| date         | string | 数据日期 |
| pushActiveUv | int    | 计划发送 |
| pushOnlineUv | int    | 实际发送 |
| verifySvcUv  | int    | 抵达设备 |
| verifyUv     | int    | 展示     |
| clickUv      | int    | 点击     |
| cleanupUv    | int    | 清除     |

#### PushStatOverviewData（iOS&macOS）

| 参数名称     | 类型   | 说明     |
| ------------ | ------ | -------- |
| date         | string | 数据日期 |
| pushActiveUv | int    | 计划发送 |
| pushOnlineUv | int    | APNs 成功接收 |
| verifySvcUv  | int    | 抵达 |
| clickUv      | int    | 点击     |



## 示例说明
#### 请求示例
```json
{
 "startDate": "20190724",
 "endDate": "20190725"
}
```

#### 应答示例
```json
{
 "retCode": 0,
 "errMsg": "NO_ERROR",
 "pushStatOverviewData": [
 {
 "date": "20190724",
 "pushActiveUv": 2,
 "pushOnlineUv": 2,
 "verifySvcUv": 2,
 "verifyUv": 2,
 "clickUv": 0,
 "cleanupUv": 0
 },
 {
 "date": "20190725",
 "pushActiveUv": 2,
 "pushOnlineUv": 2,
 "verifySvcUv": 2,
 "verifyUv": 2,
 "clickUv": 1,
 "cleanupUv": 1
 }
 ]
}
```
