## cookie 的使用和案例

#### 参考链接https://www.w3school.com.cn/js/js_cookies.asp

##### 1. 基本语法
######  1.1 方法一
    # 创建cookie 整个站点可用 name: cookie名称, value: cookie值，expiress: 有效期
    Cookies.set('name', 'value'， { expiress:4});
        
        # 设置有效期15分钟
        var inFifteenMinutes = new Date(new Date().getTime() + 15 * 60 * 1000);
        Cookies.set('foo', 'bar', {
            expires: inFifteenMinutes
        }); 
        
        # 设置有效路径, 在http://127.0.0.1/path1/路径下cookie有效
        Cookies.set('name', 'value', { expires: 4, path: '/' });
        Cookies.set('name', 'value', { expires: 4, path: 'http://127.0.0.1/path1/xxx.html' });
        
    # 读取cookie
    Cookies.get('name')
        
        # 读取所有可见cookie：返回的是个json对象；
        Cookies.get()
    
    # 删除Cookie
    Cookies.remove('name')
    
        # 删除当前页面所在路径下某个有效的cookie
        Cookies.set('name', 'value', { path: '' })
        Cookies.remove('name') // fail!
        Cookies.remove('name', { path: '' }) // removed!
        
    # cookie是否编码
    $.cookie.raw = true;
    
    # 是否以json格式进行存储和读取
    $.cookie.json = true;
    
###### 1.2 方法二
    # 创建cookie
    document.cookie = "username=Bill Gates";
        
        # 添加有效日期（UTC 时间）。默认情况下，在浏览器关闭时会删除 cookie
        document.cookie = "username=John Doe; expires=Sun, 31 Dec 2017 12:00:00 UTC";
        
    # 读取cookie 会在一条字符串中返回所有 cookie，比如：cookie1=value; cookie2=value; cookie3=value
    var x = document.cookie;
    
    # 修改cookie
    document.cookie = "username=Steve Jobs; expires=Sun, 31 Dec 2017 12:00:00 UTC; path=/";
    
    # 删除cookie 直接把 expires 参数设置为过去的日期即可
    document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 UTC; path=/;";
    
##### 2 封装cookie的增删改查的函数
    function setCookie(key, value, iDay) {
        var oDate = new Date();
        oDate.setDate(oDate.getDate() + iDay);
        document.cookie = key + '=' + value + ';expires=' + oDate;
    }
    
    function removeCookie(key) {
        setCookie(key, '', -1);//这里只需要把Cookie保质期退回一天便可以删除
    }
    
    function getCookie(key) {
        var cookieArr = document.cookie.split('; ');
        for(var i = 0; i < cookieArr.length; i++) {
            var arr = cookieArr[i].split('=');
            if(arr[0] === key) {
                return arr[1];
            }
        }
        return false;
    }