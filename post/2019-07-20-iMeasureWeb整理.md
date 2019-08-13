## laydate控件系列

- html

  加两个带id的输入框

- js

  ```js
  //输入框默认显示今天零点-now
  $("#startTime").val(laydate.now(0, 'YYYY-MM-DD 00:00:00'));
  $("#endTime").val(laydate.now(0, 'YYYY-MM-DD hh:mm:ss'));
  laydate({
  	elem: "#startTime",
  	istime: true,
  	format: "YYYY-MM-DD hh:mm:ss"
  });
  laydate({
  	elem: "#endTime",
  	//	value: new Date(),英文年月 不行
  	istime: true,
  	format: "YYYY-MM-DD hh:mm:ss"
  });
  ```

## bootstrap table系列

1. #### 文件导出

   - ##### 方法一：自定义导出按钮

     - 引入文件

     <script th:src="@{plugins/bootstrap/export/tableExport.js}"></script>
<script th:src="@{plugins/bootstrap/export/tableExport_t.js}"></script>
     - 在html自定义导出按钮
    
     <button type="button" class="btn btn-default btn-sm btnTT" id="table_reportExcel" data-type="excel" style="font-size: 14px;margin-top:-2px;outline:none;">导出</button>
    
     > 导出按钮的id有格式在文件【tableExport_t.js】中声明，$("#"+that.$el.attr("id")+"_reportExcel").unbind().bind—这个"id"是与之关联的table的id
    
     - js中配置showtable()
    
     ```javascript
height: $(window).height() - 200,
     showExport: false,
     exportDataType: "all",
     exportTypes: ['excel'],
     toolbarAlign: 'right', //toolbar位置  
     toolbar: "#toolbar",//必要
     exportOptions: {
      	fileName: curF, //文件名称设置 '产品A-检测项1'
      },
     ```
     
     ```

   - ##### 方法二：bootstrap export自带

     - 引入文件

       <script th:src="@{plugins/bootstrap/export/tableExport.js}"></script>
<script th:src="@{plugins/bootstrap/export/bootstrap-table-export.js}"></script>
- 对源码中按钮新增定义其id=“exportOut”，来使其显示在所需要的位置【*~~仍存在问题下拉选中excel类型的位置不对~~*】
  
  ```css
       #exportOut{
    	margin-top: -111%;
       	margin-right: 10px;
       }//使这个按钮保持与日期查询在同一行
  ```
  
     
  
2. #### 鼠标单击选中行变色

   **在showtbale()配置中**

   ```js
   onClickRow:function (row,$element) {//选中行变色
       	  $('.info').removeClass('info');
             $($element).addClass('info');
   },
   ```

   

3. #### 鼠标悬停在内容过长的单元格显示详情

   ```js
   {
   					field: "jcsjRemark",
   					title: "备注",
   					align: "center",
   					valign: "middle",
   					width: 400,
   					class:'colStyle1',
   					formatter: paramsMatter
   },
   function paramsMatter(value, row, index) {//新建DOM节点 用到‘title’
   	 var values = row.jcsjRemark;
   	 console.log(values);
        var span=document.createElement('span');
        span.setAttribute('title',values);
        span.innerHTML = row.jcsjRemark;
        return span.outerHTML;
   }
   ```

   ```css
   .colStyle1{
     	text-overflow: ellipsis;
       overflow: hidden;
       white-space: nowrap;
     }
   ```

4. #### 在不同分辨率下表格宽度自适应不出现横向滚动条

   **columns: []内不要设置width：200之类的**

   ---

- #### 其余注意点

  - bootstrap初始化中的顺序安排
  - ...

## MetisMenu菜单系列

**设置菜单栏滚动（不显示滚动条）**

html中定义一个`class=“toHideFlow”`添加style`Overflow-y:scroll;overflow-x:hidden;`

```css
.toHideFlow::-webkit-scrollbar {//隐藏滚动条，在firefox、ie失效
    display: none;
}
```

## jQuery选择器

- 通过点击取值对标签新增一个onclick带参数事件，onclick='getId(this)'，     data-upper="` +
  data.rows[i].detections[j].jcxmUpper + `" 加在单字符串内时需要“ ”的引用，正常情况下不需要“``”。``

  > 其中，``为ES6新增，可在其内部输入换行字符串。

- 链式引用

```js
         		//动态加载覆盖菜单原有的信息。prev兄弟级 parent取上一级，上上级写法	$(get1).parent().parent().data("showname", $("#productName").val());
$(get1).prev().html($("#productName").val());
				// $(get1).prev().html($("#showProductName").val());

				// $(get1).parent().html($("#productName").val());
				//修改父元素data-showname的内容 ，点击更新右栏检测项目名称
$(get1).parent().data("showname", $("#productName").val());
				//				console.log($(get1).data('showname'));
				// 模态框内value也动态更新
$(get1).data("name", $("#productName").val());
$(get1).data("type", $("#productType").val());
$("#showProductName").html($("#productName").val());
```

## Handsontable配置系列

```js
			data : kztData,
			columns : headArr,
			stretchH : 'none',// 拉伸模式 all 均匀拉伸 、 none 不拉伸
			autoWrapRow : true,
			manualRowResize : true,// 行拉伸
			manualColumnResize : true,// 列拉伸
			mergeCells : false,// 合并单元格
			copyPaste : true,// 复制粘贴
			colWidths: colwidth,
			rowHeights: 20,
			rowHeaders :  function(index) { return public_offset+index+1 },// 左侧序号列【容易闪烁 不好用】
			colHeaders : ColHeaders,// 表头字段名称
			manualRowMove : false,// 行拖动
			manualColumnMove : false,// 列拖动
			className : "htCenter htMiddle",// 居中样式
```



## 根据窗口尺寸动态计算table展示的行数

```js
var colwidth = $(".hot_table").width()/3-23;//动态计算列宽以适应不同分辨率而不显示横向滚动条，3列
public_limit = parseInt(($(window).height() - 190 )/30);//全局变量limit为服务器分页传过去的参数，记录每页的行数。offset是每页的第一行索引。当前每行宽30px
```



## 分页插件

“第startNums条到第endNums条记录，总共countPage条记录”

```js
function getPageFormatter(){
    $("#countPage").html(public_total);
	$("#startNums").html(public_offset+1);
	if(public_offset+hot.countRows()>public_total){
		$("#endNums").html(public_total);
	}else{
		$("#endNums").html(public_offset+hot.countRows());
	}
	if(public_offset+1>public_total){
		$("#startNums").html(public_offset);
	}else{
		$("#startNums").html(public_offset+1);
	}
}
```

分页传参

！在调查询接口时成功事件中必须重新给`public_total`赋值

```js
		success: function (data) {
			public_total = data.total;
			kztData = data.rows;
			...
		}
```

```js
function initTableAndPage(){
	$.ajax({
		url: "detectionData/findDataByDetectionItemId.action",
		type: "GET",
		dataType: "JSON",
		async: true,
		data: {
			detectionItemId: detectionId,
			limit: public_limit,
			offset: 0
		},
		success: function (data) {
			var total = data.total;
			if (total < public_limit) {
				public_count = 1;
			} else {
				if (total % public_limit == 0) {			
					public_count = (total / public_limit) + 1;		
				} else {
					public_count = Math.ceil(total / public_limit);
				}
			}		
			public_offset = (public_count - 1) * public_limit;
			p_z1();
			nomarlGetTable();		
		},
		error: function (data) {
			toastr.error(data.message);
		},
	});
}
```

分页插件配置

```js
function p_z1() {
	$("#pagination3").pagination({
	   currentPage: public_count,// 当前页数
	   totalPage: public_count,// 总页数
	   isShow: true,// 是否显示首尾页
	   count: 5,// 显示个数
	   homePageText: "首页",// 首页文本
	   endPageText: "尾页",// 尾页文本
	   prevPageText: "上一页",// 上一页文本
	   nextPageText: "下一页",// 下一页文本
	   callback: function(current) {
	       // 回调,current(当前页数)
		   console.log(current)
			public_page = current;
			public_offset = (current - 1) * public_limit;			
			if(pdSearchAndLl==0){ //0录入  1查询
				console.log("000",pdSearchAndLl);
				nomarlGetTable();
			}else{
				console.log("111",pdSearchAndLl);
				searchInitTableT();
			}
	   }
	});
}
```



## 判断机器输入与是手动输入

先记录第一个数字键按下的时间t1，再判断单元格修改完成时的时间t2，根据两者时间差diff来判断是机器输入还是手动输入

```js
beforeKeyDown: function (e) {//键盘侦听事件
			countTemp = 1;//可以当成无用
			if (!flags && (e.keyCode > 47 && e.keyCode < 58) || (e.keyCode > 95 && e.keyCode < 106)) {//判断是否是数字键
				flags = true;
				tempTime1 = new Date().getTime();
			}

			if (e.keyCode == 13) {//判断是否是回车键
				flags = false;
				tempTime2 = new Date().getTime();
				var diff = tempTime2 - tempTime1;
				countTemp = 1;
				if (diff > 200) {
//					toastr.warning("请不要手动修改数据");
					flagchange = false;
				}
			}
}
```

```js
afterChange: function (changes, source) {// 单元格修改后事件
			flags = false;
			tempTime2 = new Date().getTime();
			var diff = tempTime2 - tempTime1;
			console.log("tempTime1", tempTime1);
			console.log("tempTime2", tempTime2);
			console.log("diff", diff);
			tempTime1 = 0;
			tempTime2 = 0;
			countTemp = 1;
			if (diff > 350) {//handsontable中点击事件与回车功能等同
				toastr.warning("请不要手动修改数据");
				flagchange = false;
			}
		//以下是光标重定位部分
          var obj = {
            id: "fatftft"
          };
          var s = hot.setCellMetaObject(xx1, 1, obj);
          if (!flagchange) {
            flagchange = true;
            var rowData = hot.getSourceDataAtRow(xx1);
            rowData.jcsjData = "";
            hot.render();
            setTimeout(function() {
              xx1 = hot.countRows();
              xx2 = xx1;
              yy1 = 1;
              yy2 = 1;
              hot.selectCell(xx1 - 1, yy1, xx2 - 1, yy2);
            }, 10);
          } else {
            var str = changes[0][1];
            var strx = changes[0][1];
            kztIndex = changes[0][0];

            var rowData = hot.getSourceDataAtRow(changes[0][0]);
            var hangshu = 0;
            var hangshuju;
            hangshu = hot.countRows();
            console.log("几行啊", hangshu, changes[0][0]);
            saveFormat();
          }
	}
}
```



## 判断js控件是否载入完成

```js
document.onreadystatechange = subSomething;    　　　 //当页面加载状态改变的时候执行这个方法
function subSomething(){
    if(document.readyState == "complete") {         //判断页面加载状态
        console.log("判断加载完成");
        $('#showOnload').modal('hide');
    }else{
	   	$('#showOnload').modal('show');
    }
}
```

**前端模态框**

```html
<div class="modal fade" id="showOnload" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true" style="">
    <div class="modal-dialog modal-dialog-centered" style="width:330px;padding-top:calc(25vh )">
      <div class="modal-content" style="border-radius: 5px;">
        <div class="modal-header text-center" style="font-weight:bold;border-radius: 5px;" >
          <span>正在加载中，请稍等......</span>
        </div>
         <div class="modal-body" >
            //loading动画效果
 			<div class="spinner">
			  <div class="spinner-container container1">
			    <div class="circle1"></div>
			    <div class="circle2"></div>
			    <div class="circle3"></div>
			    <div class="circle4"></div>
			  </div>
			  <div class="spinner-container container2">
			    <div class="circle1"></div>
			    <div class="circle2"></div>
			    <div class="circle3"></div>
			    <div class="circle4"></div>
			  </div>
			  <div class="spinner-container container3">
			    <div class="circle1"></div>
			    <div class="circle2"></div>
			    <div class="circle3"></div>
			    <div class="circle4"></div>
			  </div>
			</div>
        </div>
      </div><!-- /.modal-content -->
    </div><!-- /.modal -->
  </div>
```

**css实现**

## CSS样式系列

- vh vw ：相对于视口的高度。视口被均分为100单位的vh 

  `height:calc(100% - 10px)`

- 清除input中下拉框内的缓存

  `autocomplete=”off”`

- 禁止textarea拉伸

  `style=”resize:none”`

- ...

- 

- 

## js加载问题

- <script th:src="@{plugins/handsontable/js/handsontable.full.js}"></script>
替换为
  
<script type="text/javascript">
  document.write('<th:script src=\''@{plugins/handsontable/js/handsontable.full.js}\"></script>')</script>

加载速度大概快三分之一

- **异步加载**：把占用时间长的控件引入放在最后，并改成异步加载

  ```html
  <script type="text/javascript">document.write("<scr"+"ipt async src=\"plugins/handsontable/js/handsontable.full.js\"></sc"+"ript>");
  </script>
  ```

  