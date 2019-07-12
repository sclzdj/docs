## JS常用函数库

#### inArray

##### 判断是否在数组中

```javascript
/**
 * 判断是否在数组中
 * @param item 判断条目
 * @param arr 数组
 * @returns bool
 */
function inArray(item, arr) {
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
    var beforeUrl = '';//页面主地址（参数之前地址）
    if (url.indexOf("?") != -1) {
        beforeUrl = url.substr(0, url.indexOf("?"));
    }else{
        return url;
    }
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

#### postJump

##### 模拟form表单的post跳转

```javascript
/*
 * post跳转，模拟form表单的提交
 * @param target 跳转地址
 * @param params 参数 {name:'sclzdj',phone:'18353621790'}
 * @param target 跳转方式：_self,_blank,不传默认_self
 */
function postJump(url, params, target) {
    if (target === undefined) {
        target = '_self';
    }
    //创建form表单
    var temp_form = document.createElement("form");
    temp_form.action = url;
    temp_form.target = target;
    temp_form.method = "post";
    temp_form.style.display = "none";
    //添加参数
    for (var key in params) {
        var opt = document.createElement("textarea");
        opt.name = key;
        opt.value = params[key];
        temp_form.appendChild(opt);
    }
    document.body.appendChild(temp_form);
    //提交数据
    temp_form.submit();
}
```

#### serializeArrayToSubmitData

##### 序列表单数据转提交对象

```javascript
/**
 * 序列表单数据转提交对象
 * *** require{jQuery}
 * @param formData jquery用serializeArray方法序列化的表单数据
 */
function serializeArrayToSubmitData(formData){
    var submitData = {};
    $(formData).each(function (k, v) {
        //下面是把多选项重组
        if (v.name.indexOf("[]") != -1) {
            v.name = v.name.replace("[]", "");
            if (submitData[v.name] === undefined) {
                submitData[v.name] = [];
            }
            submitData[v.name].push(v.value);
        } else {
            submitData[v.name] = v.value;
        }
    });
}
```

