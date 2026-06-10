# Ajax测试接口文档

> baseURL：http://123.207.32.32:9060/beike/api



## 一. Ajax测试接口

### 1. 获取书籍列表数据

说明 : 调用此接口 , 可获取书籍列表的数据

**请求方法 :** POST

**请求头支持:** 

```json
Content-Type: 'application/x-www-form-urlencoded;charset=UTF-8'
# or
Content-Type: 'application/json;charset=UTF-8'
```

**接口地址 :** `/listGoods`

**必选参数 :** 

```json
page  : 第几页
count : 每也多少条数据
```

**调用例子 :** `baseURL + /listGoods`

**接口返回的数据格式：**

```json
{
    "code": 200,
    "data": {
        "books": [
            {
                "name": "《深入浅出Node.js》 第0版",
                "id": 0,
                "descp": "京东JD.COM图书频道为您提供《深入浅出Node.js(图灵出品)》在线选购，本书作者：，出版社：人民邮电出版社。买图书，到京东。网购图书，享受最低优惠折扣!",
                "price": 120,
                "count": 958
            },
            {
                "name": "《深入浅出Node.js》 第1版",
                "id": 1,
                "descp": "京东JD.COM图书频道为您提供《深入浅出Node.js(图灵出品)》在线选购，本书作者：，出版社：人民邮电出版社。买图书，到京东。网购图书，享受最低优惠折扣!",
                "price": 171,
                "count": 513
            },
        ]
    }
}
```



### 2. 获取推荐书籍

说明 : 调用此接口 , 获取推荐书籍列表

**请求方法 :** GET 

**接口地址 :** `/recommendGoods`

**必选参数 :** 无

**调用例子 :** `baseURL + /recommendGoods`

```json
{
    "code": 200,
    "data": {
        "books": [
            {
                "name": "《深入浅出Node.js》 第0版",
                "id": 0,
                "descp": "京东JD.COM图书频道为您提供《深入浅出Node.js(图灵出品)》在线选购，本书作者：，出版社：人民邮电出版社。买图书，到京东。网购图书，享受最低优惠折扣!",
                "price": 120,
                "count": 958
            },
            {
                "name": "《深入浅出Node.js》 第1版",
                "id": 1,
                "descp": "京东JD.COM图书频道为您提供《深入浅出Node.js(图灵出品)》在线选购，本书作者：，出版社：人民邮电出版社。买图书，到京东。网购图书，享受最低优惠折扣!",
                "price": 171,
                "count": 513
            },
        ]
    }
}
```



### 3. 获取书籍详情

说明 : 调用此接口 , 获取书籍详情

**请求方法 :** GET 

**接口地址 :** `/recommendGoodDetail/{id}`

**必选参数 :** `id ` 为 0  到 199

**调用例子 :** `baseURL + /recommendGoodDetail/199`

**接口返回的数据格式：**

```json
{
    "code": 200,
    "data": {
        "book": {
            "name": "《深入浅出Node.js》 第199版",
            "id": 199,
            "descp": "京东JD.COM图书频道为您提供《深入浅出Node.js(图灵出品)》在线选购，本书作者：，出版社：人民邮电出版社。买图书，到京东。网购图书，享受最低优惠折扣!",
            "price": 192,
            "count": 184
        }
    }
}
```



