## JS常用函数库

#### inArray

##### 判断是否在数组中

```javascript
/**
 * 判断是否在数组中
 * @param arr 数组
 * @param item 判断条目
 * @returns bool
 */
function inArray(arr, item) {
    var i = arr.length;
    while (i--) {
        if (arr[i] === item) {
            return true;
        }
    }
    return false;
}
```

#### isArray

##### 判断是否是数组

```javascript
/**
 * 判断是否是数组
 * @param value 判断值
 * @returns bool
 */
function isArray(value){
    if (typeof Array.isArray === "function") {
        return Array.isArray(value);
    }else{
        return Object.prototype.toString.call(value) === "[object Array]";
    }
}
```

#### delUrlParam 

##### 删除链接地址的某些参数

```javascript
/**
 * 删除链接地址的某些参数
 * *** include{inArray,isArray}
 * @param paramKey 删除的参数名称，多个可用数组
 * @param url 链接地址，不传默认为当前页面url
 * @returns url 处理后的链接地址
 */
function delUrlParam(paramKey,url) {
    if(url===undefined){
        url = window.location.href;    //页面url
    }
    var urlParam = window.location.search.substr(1);   //页面参数
    var beforeUrl = url.substr(0, url.indexOf("?"));   //页面主地址（参数之前地址）
    var nextUrl = "";

    var arr = new Array();
    if (urlParam != "") {
        var urlParamArr = urlParam.split("&"); //将参数按照&符分成数组
        for (var i = 0; i < urlParamArr.length; i++) {
            var paramArr = urlParamArr[i].split("="); //将参数键，值拆开
            //如果和要删除的不一致，则加入到参数中
            if(isArray(paramKey)){
                if (!inArray(paramKey, paramArr[0])) {
                    arr.push(urlParamArr[i]);
                }
            }else{
                if (paramArr[0] != paramKey) {
                    arr.push(urlParamArr[i]);
                }
            }
        }
    }
    if (arr.length > 0) {
        nextUrl = "?" + arr.join("&");
    }
    url = beforeUrl + nextUrl;
    return url;
}
```
