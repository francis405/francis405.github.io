# js 获取当前与一个月前的日期
    在前端页面的日期选择框里放入当前日期以及一个月前的日期。先直接将month减去1，如果减完日期无效，例如：3月31变为2月31,，出现逻辑错误，那就采取减30天的方案

jsp代码
```markdown
<div class="searchTips" style="width: 520px;">
	<span class="tipsSpan">报废日期：</span> 
	<input id="startDate1" type="text" class="input-text radius size-S Wdate tipsIpt" placeholder=""
		onfocus="WdatePicker({dateFmt:'yyyy-MM-dd',readOnly:true,qsEnabled:false,maxDate:'#F{$dp.$D(\'startDate2\')}'})">
	<span style="float: left; margin: 0 5px;">-</span> 
	<input id="startDate2" type="text" class="input-text radius size-S Wdate tipsIpt" placeholder=""
		onfocus="WdatePicker({dateFmt:'yyyy-MM-dd',readOnly:true,qsEnabled:false,minDate:'#F{$dp.$D(\'startDate1\')}'})">
</div>
```

js代码

初始化date框
```javascript
function init_date() {
	//现在
	var time = new Date();
	var day = ('0' + time.getDate()).slice(-2);
    var month = ('0' + (time.getMonth() + 1)).slice(-2);
    var today = time.getFullYear() + '-' + month + '-' + day ;
	 $('#startDate2').val(today);
	 
	 //1.一个月前 month直接减1
	 var monthAgo = ('0' + (time.getMonth() )).slice(-2);
	 var oneMonAgo = time.getFullYear() + '-' + monthAgo + '-' + day ;
	 if(month=="01"){
		 monthAgo = 12;
		 var yearAgo = time.getFullYear()-1;
		 oneMonAgo =yearAgo + '-' + monthAgo + '-' + day ;
	 }
	 //如果日期有效
	 if(judgeDate(oneMonAgo)){
		 $('#startDate1').val(oneMonAgo); 
	 }else{
		//2.减去30天
		 var ago=new Date(time.getTime() - 30*24*60*60*1000); //30天前
		 var day = ('0' + ago.getDate()).slice(-2);
		 var month = ('0' + (ago.getMonth() + 1)).slice(-2);
		 var oneMonAgo = ago.getFullYear() + '-' + month + '-' + day ;
		 $('#startDate1').val(oneMonAgo); 
	 }
}
```
日期有效验证方法(验证某一日是否真实存在，例如2020-02-29)
```javascript
function judgeDate(date){
	const rmons = [31,29,31,30,31,30,31,31,30,31,30,31],
		  pmons = [31,28,31,30,31,30,31,31,30,31,30,31];
	var year = parseInt(date.substr(0,4)),
		mon = parseInt(date.substr(5,7)),
		day = parseInt(date.substr(8,10));
	if(year % 4 == 0 && year % 100 != 0 || year % 400 == 0){
		return mon > 0 && mon <=12 && day > 0 && day <= rmons[mon-1];
	}else{
		return mon > 0 && mon <=12 && day > 0 && day <= pmons[mon-1];
	}
}
```

**结果**
![显示](https://img-blog.csdnimg.cn/20200904133552743.png#pic_center)
**验证**
在上文的代码加日期参数
```javascript
var time = new Date("2013-03-29");
```
前端刷新后
![](https://img-blog.csdnimg.cn/2020090413425279.png#pic_center)
因为截止日期为03-29，没有02-29，就减去三十天到02-27