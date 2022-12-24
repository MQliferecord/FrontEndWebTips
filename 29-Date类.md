# Date类

几个Date对象常用的API属性：
```javascript
//获取Date类格式显示
let myDate = new Date();//Wed Dec 21 2022 16:19:50 GMT+0800 (中国标准时间)
//获取时间戳
let myDate2 = (new Date()).valueOf();//1671611103537
//获取当前年份，getYear()是获取从1970到当前年的差值，比如今年2022，getYear()结果是：122
myDate.getFullYear();//2022
//获取月份，注意得‘+1’
myDate.getMonth()+1;//12
//获取当前得日期
myDate.getDate();//21
//获取小时
myDate.getHours()//16
//获取分钟
myDate.getMinutes()//33
//获取秒数
myDate.getSeconds();//17

//1.
//将字符串形式得日期转化为日期的对象
//g是全局匹配得意思，不然找到第一个-就返回答案不再查找
//Date.parse()解析字符串变成时间戳，但是格式限制很大，无法解析xxxx-xx-xx的格式
let strTime = "2011-04-06"
let date = new Date(Date.parse(strTime.replace(/-/g,"/")))//Wed Apr 06 2011 08:00:00 GMT+0800 (中国标准时间)
//let date2 = Date.parse(strTime.replace(/-/g,"/")).getFullYear();无法解析！！

//更推荐以下办法,因为没有Date.parse()的格式限制
let date2 = new Date(strTime);//Wed Apr 06 2011 08:00:00 GMT+0800 (中国标准时间)
let date3 = new Date(strTime).getTime();//1302048000000

//会显示1970的起始时间
let year = 1999;
let date3 = new Date(year);//Thu Jan 01 1970 08:00:01 GMT+0800 (中国标准时间)


//2.
//将日期格式转化为日期的标准字符串
let formatDate = function(date){
    let y = date.getFullYear();
    let m = date.getMonth()+1;
    m = m < 10? '0'+m:m;
    let d = date.getDate();
    d = d < 10? '0'+d:d;
    let h = date.getHours();
    h = h < 10? '0'+h:h; 
    let min = date.getMinutes();
    min = min < 10? '0'+min:min;
    let s = date.getSeconds();
    s = s < 10? '0'+s:s;
    return y+'-'+m+'-'+d+' '+h+':'+min+':'+s
}
console.log(new Date())//2022-12-21 17:12:39
console.log((new Date()).valueOf())//报错


//3.
//将时间戳转化为固定日期格式字符串
let formatDate2 = function(time,format){
    let date = new Date(time);
    let f = function(i){
        return i<10? '0'+i:i;
    } 
    return format.replace(/yyyy|MM|dd|hh|mm|ss/g,function(a){
        switch(a){
            case 'yyyy':
                return date.getFullYear();
                break;
            case 'MM':
                return date.getMonth()+1;
                break;
            case 'dd':
                return date.getDate();
                break;
            case 'hh':
                return date.getHours();
                break;
            case 'mm':
                return date.getMinutes();
                break;
            case 'ss':
                return date.getSeconds();
                break;
        }
    })
}
```