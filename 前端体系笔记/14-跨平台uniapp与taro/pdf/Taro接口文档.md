# Taro接口文档

> baseURL：http://codercba.com:9060/juanpi/api

## 一. 首页接口

### 1. 获取首页Banner数据

**请求方法 :** GET 

**接口地址 :** `/homeinfo`

**调用例子 :** `baseURL + /homeinfo`

**接口返回的数据格式：**

```json
{
    "code": 200,
    "data": {
	 "xxx": "xxx"	        
    }
}    
```



### 2. 受欢迎度和推荐的接口

**请求方法 :** GET 

**接口地址 :** `/recommend`

**调用例子 :** `baseURL + /recommend`

```json
{
    "code": 200,
    "data": {
	 "xxx": "xxx"	        
    }
} 
```



### 3. 商品列表接口

**请求方法 :** POST 

**接口地址 :** `baseURL + /goods`

**可选参数（json格式） :** 

```
{
    "page": 1,  # 默认为1，即第一页
	"type": 0 # 支持 0 和 1。 其中 0 代表是精选专场， 1 代表是精选单品
} 
```

**调用例子 :** `baseURL + /goods`

**接口返回的数据格式：**

```
{
    "code": 200,
    "data": {
	 "xxx": "xxx"	        
    }
} 
```

## 二.我的接口

### 1.获取我的页面列表数据

**请求方法 :** GET 

**接口地址 :** `/profile`

**必选参数 :** 无

**调用例子 :** `baseURL + /profile`

**接口返回的数据格式：**

```json
{
    "code": 200,
    "data": {
	 "xxx": "xxx"	        
    }
} 
```



