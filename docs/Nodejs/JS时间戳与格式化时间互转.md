# JS时间戳与格式化时间互转

## Javascript 获取当前时间戳（毫秒级别）

```
// 方法一(精确到秒)
var timestamp1 = Date.parse( new Date());

// 方法二(精确到毫秒)
var timestamp2 = ( new Date()).valueOf();

// 方法三(精确到毫秒)
var timestamp3 = new Date().getTime();
```

## 时间戳转成格式化时间
```
const dateFormat = function(dateStr, fmt) {
  let date = new Date(dateStr)
  var o = {
    "Y+": date.getFullYear(),
    "M+": date.getMonth() + 1,                 //月份
    "D+": date.getDate(),                    //日
    "h+": date.getHours(),                   //小时
    "m+": date.getMinutes(),                 //分
    "s+": date.getSeconds(),                 //秒
    "q+": Math.floor((date.getMonth() + 3) / 3), //季度
    "S+": date.getMilliseconds()             //毫秒
  };
  for (var k in o) {
    if (new RegExp("(" + k + ")").test(fmt)) {
      if (k == "Y+") {
        fmt = fmt.replace(RegExp.$1, ("" + o[k]).substr(4 - RegExp.$1.length));
      }
      else if (k == "S+") {
        var lens = RegExp.$1.length;
        lens = lens == 1 ? 3 : lens;
        fmt = fmt.replace(RegExp.$1, ("00" + o[k]).substr(("" + o[k]).length - 1, lens));
      }
      else {
        fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
      }
    }
  }
  return fmt;
}
```

## 时间戳格式化成固定格式 YYYY-MM-DD hh:mm:ss

```
formatTimestamp(timestamp) {
        let date = new Date(timestamp * 1000);//时间戳为10位需*1000，时间戳为13位的话不需乘1000
        let Y = date.getFullYear() + '-';
        let M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
        let D = date.getDate() + ' ';
        let h = date.getHours() + ':';
        let m = (date.getMinutes() < 10 ? '0'  + date.getMinutes() : date.getMinutes()) + ':';
        let s = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds();
        return Y+M+D+h+m+s;
    }
```

## 格式化时间转成时间戳

```
"2016-08-03 00:00:00" ——> 1470153600000
```

```
let dateF = "2016-08-03 00:00:00"
let date = new Date(dateStr)
let dateS = date.valueOf()
console.log(dateS)  // 1470153600000
```

## JS获取UTC时间戳

```
// 获取当前UTC时间
const getUTCDate = ()=>{
  let date = new Date()
  let year = date.getUTCFullYear()
  let month = date.getUTCMonth()
  let day = date.getUTCDate()  // 返回the day of the month
  let hours = date.getUTCHours()
  let minutes = date.getUTCMinutes()
  let seconds = date.getUTCSeconds()
  let UTCDate = new Date(Date.UTC(year,month,day,hours,minutes,seconds))
  return Math.floor(UTCDate.getTime()/1000)
}

// 获取今天凌晨的UTC时间
const getZeroUTCDate = ()=>{
    let date = new Date()
    let year = date.getUTCFullYear()
    let month = date.getUTCMonth()
    let day = date.getUTCDate()  // 返回the day of the month
    let hours = date.getUTCHours()
    let minutes = date.getUTCMinutes()
    let seconds = date.getUTCSeconds()
    let UTCDate = new Date(Date.UTC(year,month,day,0,0,0))
    return Math.floor(UTCDate.getTime()/1000)
}
```


## 参考文章

- [JavaScript获取时间戳与时间戳转化](https://segmentfault.com/a/1190000006160703)
- [如何获取UTC时间戳JavaScript？](https://codeday.me/bug/20170627/29413.html)