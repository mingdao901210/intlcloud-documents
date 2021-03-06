## 简介

本文档提供关于存储桶的基本操作和访问控制列表（ACL）的相关 API 概览以及 SDK 示例代码。

**基本操作**

| API                                                          | 操作名             | 操作描述                           |
| ------------------------------------------------------------ | ------------------ | ---------------------------------- |
| [GET Service](https://intl.cloud.tencent.com/document/product/436/8291) | 查询存储桶列表     | 查询指定账号下所有的存储桶列表     |
| [PUT Bucket](https://intl.cloud.tencent.com/document/product/436/7738) | 创建存储桶         | 在指定账号下创建一个存储桶         |
| [HEAD Bucket](https://intl.cloud.tencent.com/document/product/436/7735) | 检索存储桶及其权限 | 检索存储桶是否存在且是否有权限访问 |
| [DELETE Bucket](https://intl.cloud.tencent.com/document/product/436/7732) | 删除存储桶         | 删除指定账号下的空存储桶           |

**访问控制列表**

| API                                                          | 操作名         | 操作描述                       |
| ------------------------------------------------------------ | -------------- | ------------------------------ |
| [PUT Bucket acl](https://intl.cloud.tencent.com/document/product/436/7737) | 设置存储桶 ACL | 设置指定存储桶访问权限控制列表 |
| [GET Bucket acl](https://intl.cloud.tencent.com/document/product/436/7733) | 查询存储桶 ACL | 查询存储桶的访问控制列表       |

## 基本操作

### 查询存储桶列表

#### 功能说明

用于查询指定账号下所有的存储桶列表。

#### 方法原型

```cpp
CosResult GetService(const GetServiceReq& request, GetServiceResp* response)
```

#### 请求示例

```cpp
qcloud_cos::CosConfig config("./config.json");
qcloud_cos::CosAPI cos(config);

std::string bucket_name = "examplebucket-1250000000";

qcloud_cos::GetServiceReq req;
qcloud_cos::GetServiceResp resp;
qcloud_cos::CosResult result = cos.GetService(req, &resp);

// 调用成功，调用 resp 的成员函数获取返回内容
if (result.IsSucc()) {
    const qcloud_cos::Owner& owner = resp.GetOwner();
    const std::vector<qcloud_cos::Bucket>& buckets = resp.GetBuckets();
    std::cout << "owner.m_id=" << owner.m_id << ", owner.display_name=" << owner.m_display_name << std::endl;               
     for (std::vector<qcloud_cos::Bucket>::const_iterator itr = buckets.begin(); itr != buckets.end(); ++itr) {
     	const qcloud_cos::Bucket& bucket = *itr;
     	std::cout << "Bucket name=" << bucket.m_name << ", location=" 
     	<< bucket.m_location << ", create_date=" << bucket.m_create_date << std::endl;                                  
     } 
} else {
    // 可以调用 CosResult 的成员函数输出错误信息，如 requestID 等
} 
```

#### 参数说明

| 参数 | 参数描述                              |
| ---- | ------------------------------------- |
| req  | GetServiceReq，GetService 操作的请求  |
| resp | GetServiceResp，GetService 操作的返回 |

GetServiceResp 提供以下成员函数，用于获取 Get Service 返回的 XML 格式中的具体内容。 

```C++
// 获取 Bucket 持有者的信息
Owner GetOwner() const;
// 获取所有 Bucket 列表信息
std::vector<Bucket> GetBuckets() const
```

其中 Owner 的定义如下：

```cpp
struct Owner {
	std::string m_id;	// 存储桶所有者的 ID
	std::string m_display_name; // 存储桶所有者的名字信息
}; 
```

其中 Bucket 的定义如下：

```cpp
// 描述单个 Bucket 的信息                                                                                                                                                                    
struct Bucket {
	std::string m_name; // Bucket 名称
	std::string m_location; // Bucket 所在地域
	std::string m_create_date; // Bucket 创建时间。ISO8601 格式，例如 2016-11-09T08:46:32.000Z
};
```



### 创建存储桶

#### 功能说明

创建一个存储桶（PUT Bucket）。

#### 方法原型

```cpp
CosResult PutBucket(const PutBucketReq& req, PutBucketResp* resp)
```

#### 请求示例

```cpp
qcloud_cos::CosConfig config("./config.json");
qcloud_cos::CosAPI cos(config);

std::string bucket_name = "examplebucket-1250000000";

qcloud_cos::PutBucketReq req(bucket_name);
qcloud_cos::PutBucketResp resp;
qcloud_cos::CosResult result = cos.PutBucket(req, &resp);

// 调用成功，调用 resp 的成员函数获取返回内容
if (result.IsSucc()) {
    // ...
} else {
    // 可以调用 CosResult 的成员函数输出错误信息，如 requestID 等
} 
```

#### 参数说明

| 参数 | 参数描述                            |
| ---- | ----------------------------------- |
| req  | PutBucketReq，PutBucket 操作的请求  |
| resp | PutBucketResp，PutBucket 操作的返回 |

PutBucketReq 提供以下成员函数：

```C++
// 定义 Bucket 的 ACL 属性,有效值：private,public-read-write,public-read
// 默认值：private
void SetXCosAcl(const std::string& str);

// 赋予被授权者读的权限.格式：x-cos-grant-read: id=" ",id=" ".
// 当需要给子账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<SubUin>"
// 当需要给根账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>"
void SetXCosGrantRead(const std::string& str);

// 赋予被授权者写的权限,格式：x-cos-grant-write: id=" ",id=" "./
// 当需要给子账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<SubUin>",
// 当需要给根账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>"
void SetXCosGrantWrite(const std::string& str);
    
// 赋予被授权者读写权限.格式：x-cos-grant-full-control: id=" ",id=" ".
// 当需要给子账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<SubUin>",
// 当需要给根账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>"
void SetXCosGrantFullControl(const std::string& str);
```

### 检索存储桶及其权限

#### 功能说明

检索存储桶是否存在且是否有权限访问。

#### 方法原型

```C++
CosResult HeadBucket(const HeadBucketReq& req, HeadBucketResp* resp)
```

#### 请求示例

```C++
qcloud_cos::CosConfig config("./config.json");
qcloud_cos::CosAPI cos(config);

std::string bucket_name = "examplebucket-1250000000";

qcloud_cos::HeadBucketReq req(bucket_name);
qcloud_cos::HeadBucketResp resp;
qcloud_cos::CosResult result = cos.HeadBucket(req, &resp);

// 调用成功，调用 resp 的成员函数获取返回内容
if (result.IsSucc()) {
    // ...
} else {
    // 可以调用 CosResult 的成员函数输出错误信息，如 requestID 等
} 
```

#### 参数说明

| 参数 | 参数描述                              |
| ---- | ------------------------------------- |
| req  | HeadBucketReq，HeadBucket 操作的请求  |
| resp | HeadBucketResp，HeadBucket 操作的返回 |

### 删除存储桶

#### 功能说明

删除指定账号下的空存储桶。

#### 方法原型

```cpp
CosResult DeleteBucket(const DeleteBucketReq& req, DeleteBucketResp* resp)
```

#### 请求示例

```cpp
qcloud_cos::CosConfig config("./config.json");
qcloud_cos::CosAPI cos(config);

std::string bucket_name = "examplebucket-1250000000";

// DeleteBucketReq 的构造函数需要传入 bucket_name
qcloud_cos::DeleteBucketReq req(bucket_name);
qcloud_cos::DeleteBucketResp resp;
qcloud_cos::CosResult result = cos.DeleteBucket(req, &resp);

// 调用成功，调用 resp 的成员函数获取返回内容
if (result.IsSucc()) {
    // ...
} else {
    // 可以调用 CosResult 的成员函数输出错误信息，如 requestID 等
} 
```

#### 参数说明

| 参数 | 参数描述                                 |
| ---- | ---------------------------------------- |
| req  | DeleteBucketReq，DeleteBucket 操作的请求 |
| resp | DeletBucketResp，DeletBucket 操作的返回  |

## 访问控制列表

### 设置存储桶 ACL

#### 功能说明

设置指定存储桶访问权限控制列表。

#### 方法原型

```cpp
CosResult PutBucketACL(const DPutBucketACLReq& req, PutBucketACLResp* resp)
```

#### 请求示例

```cpp
qcloud_cos::CosConfig config("./config.json");
qcloud_cos::CosAPI cos(config);

std::string bucket_name = "examplebucket-1250000000";

// PutBucketACLReq 的构造函数需要传入 bucket_name
qcloud_cos::PutBucketACLReq req(bucket_name);
qcloud_cos::ACLRule rule;
rule.m_id = "123";
rule.m_allowed_headers.push_back("x-cos-meta-test");
rule.m_allowed_origins.push_back("http://www.qq.com");
rule.m_allowed_origins.push_back("http://intl.cloud.tentent.com");
rule.m_allowed_methods.push_back("PUT");
rule.m_allowed_methods.push_back("GET");
rule.m_max_age_secs = "600";
rule.m_expose_headers.push_back("x-cos-expose");
req.AddRule(rule);

qcloud_cos::PutBucketACLResp resp;
qcloud_cos::CosResult result = cos.PutBucketACL(req, &resp);

// 调用成功，调用 resp 的成员函数获取返回内容
if (result.IsSucc()) {
    // ...
} else {
    // 设置 ACL，可以调用 CosResult 的成员函数输出错误信息，比如 requestID 等
} 
```

#### 参数说明

| 参数 | 参数描述                                  |
| ---- | ----------------------------------------- |
| req  | PutBucketACLReq，PutBucketACL 操作的请求  |
| resp | PutBucketACLResp，PutBucketACL 操作的返回 |

PutBucketACLReq 提供以下成员函数：

```
// 定义 Bucket 的 ACL 属性,有效值：private,public-read-write,public-read
// 默认值：private
void SetXCosAcl(const std::string& str);

// 赋予被授权者读的权限.格式：x-cos-grant-read: id=" ",id=" ".
// 当需要给子账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<SubUin>"
// 当需要给根账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>"
void SetXCosGrantRead(const std::string& str);

// 赋予被授权者写的权限,格式：x-cos-grant-write: id=" ",id=" "./
// 当需要给子账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<SubUin>",
// 当需要给根账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>"
void SetXCosGrantWrite(const std::string& str);

// 赋予被授权者读写权限.格式：x-cos-grant-full-control: id=" ",id=" ".
// 当需要给子账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<SubUin>",
// 当需要给根账户授权时,id="qcs::cam::uin/<OwnerUin>:uin/<OwnerUin>"
void SetXCosGrantFullControl(const std::string& str);

// Bucket 持有者 ID
void SetOwner(const Owner& owner);

// 设置被授权者信息与权限信息
void SetAccessControlList(const std::vector<Grant>& grants);

// 添加单个 Bucket 的授权信息
void AddAccessControlList(const Grant& grant);
        

```

> !SetXCosAcl、SetXCosGrantRead、SetXCosGrantWrite、SetXCosGrantFullControl 这类接口与 SetAccessControlList、AddAccessControlList 不可同时使用。因为前者实际是通过设置 HTTP Header 实现，而后者是在 Body 中添加了 XML 格式的内容，二者只能二选一。 SDK 内部优先使用第一类。

ACLRule 定义如下：

```
struct Grantee {
    // type 类型可以为 RootAccount， SubAccount
    // 当 type 类型为 RootAccount 时，可以在 id 中 uin 填写帐号 ID，也可以用 anyone（指代所有类型用户）代替 uin/<OwnerUin> 和 uin/<SubUin>
    // 当 type 类型为 RootAccount 时，uin 代表主账号，Subaccount 代表子账号
    std::string m_type; 
    std::string m_id; // qcs::cam::uin/<OwnerUin>:uin/<SubUin>
    std::string m_display_name; // 非必选
    std::string m_uri;
};

struct Grant {
    Grantee m_grantee; // 被授权者资源信息
    std::string m_perm; // 指明授予被授权者的权限信息，枚举值：READ，WRITE，FULL_CONTROL
};
```

### 查询存储桶 ACL

#### 功能说明

查询存储桶的访问控制列表。

#### 方法原型

```cpp
CosResult GetBucketACL(const DGetBucketACLReq& req, GetBucketACLResp* resp)
```

#### 请求示例

```cpp
qcloud_cos::CosConfig config("./config.json");
qcloud_cos::CosAPI cos(config);

std::string bucket_name = "examplebucket-1250000000";

// GetBucketACLReq 的构造函数需要传入 bucket_name
qcloud_cos::GetBucketACLReq req(bucket_name);
qcloud_cos::GetBucketACLResp resp;
qcloud_cos::CosResult result = cos.GetBucketACL(req, &resp);

// 调用成功，调用 resp 的成员函数获取返回内容
if (result.IsSucc()) {
    // ...
} else {
    // 获取 ACL 失败，可以调用 CosResult 的成员函数输出错误信息，比如 requestID 等
} 
```

#### 参数说明

| 参数 | 参数描述                                  |
| ---- | ----------------------------------------- |
| req  | GetBucketACLReq，GetBucketACL 操作的请求  |
| resp | GetBucketACLResp，GetBucketACL 操作的返回 |

GetBucketACLResp 提供以下成员函数：

```
std::string GetOwnerID();
std::string GetOwnerDisplayName();
std::vector<Grant> GetAccessControlList();
```
