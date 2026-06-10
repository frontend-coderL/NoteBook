# uni-app接口文档

> baseURL： http://152.136.185.210:7878/api/hy66



## 一. 首页接口

### 1. 获取首页Banner等数据

**请求方法 :** GET 

**接口地址 :** `/home/multidata`

**调用例子 :** `baseURL + /home/multidata`

**接口返回的数据格式：**

```json
{
    "_id": "5bac3f5651aef21ad4c68e3c",
    "data": {
	 "xxx": "xxx"	        
    }
}    
```



### 2. 首页商品列表

**请求方法 :** GET 

**接口地址 :** `/home/data`

**调用例子 :** `baseURL + /home/data?type=pop&page=1`

**必选参数 : **

- type  商品类型，支持pop、new、sell。分别表示 ：流行、新款、精选对应的商品列表数据
- page  当前页面，从 1 开始

```json
{
    "code": 200,
    "data": {
	 "xxx": "xxx"	        
    }
} 
```





## 二.首页详情接口

### 1.详情页面数据

**请求方法 :** GET 

**接口地址 :** `/detail`

**必选参数 : **

- iid  商品的iid

**调用例子 :** `baseURL + /detail?iid=1m70y5k`

**接口返回的数据格式：**

```json
{
    "iid": "1m70y5k",
    "result": {
	 "xxx": "xxx"	        
    }
} 
```



### 2.详情页面推荐列表数据

**请求方法 :** GET 

**接口地址 :** `/recommend`

**调用例子 :** `baseURL + /recommend`

**接口返回的数据格式：**

```json
{
    "_id": "5bcc483349aab96116fac7b0",
    "data": {
	 "xxx": "xxx"	        
    }
} 
```



## 三.分类页面接口

### 1.一级菜单分类接口

**请求方法 :** GET 

**接口地址 :** `/category`

**必选参数 : **

- iid  商品的iid

**调用例子 :** `baseURL + /category`

**接口返回的数据格式：**

```json
{
    "_id": "5bcc483349aab96116fac7b0",
    "data": {
	 "xxx": "xxx"	        
    }
} 
```



### 2.二级菜单分类接口

**请求方法 :** GET 

**接口地址 :** `/subcategory`

**必选参数 : **

- maitKey  一级菜单的 maitKey

**调用例子 :** `baseURL + /subcategory?maitKey=3627`

**接口返回的数据格式：**

```json
{
    "_id": "5bcc483349aab96116fac7b0",
    "data": {
	 "xxx": "xxx"	        
    }
} 
```



### 3.二级菜下商品列表接口

**请求方法 :** GET 

**接口地址 :** `/subcategory/detail`

**必选参数 : **

- miniWallkey  一级菜单的 miniWallkey
- type  商品类型，支持pop、new、sell。分别表示 ：综合、新品、销量对应的商品列表数据

**调用例子 :** `baseURL + /subcategory/detail?miniWallkey=10062603&type=pop`

**接口返回的数据格式：**

```json
[
    {
        iid: "1kljr06", 		           
        itemMarks: "1525 1232",
        orgPrice: 42.8,
        price: 29.96,
        ...
    }
]
```



