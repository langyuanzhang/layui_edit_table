# layui_edit_table
layui表格，可编辑单元格 和 表格内加下拉框 （在lotusadmin中的运用）


```php
{extend name='public/base'}
{block name='content'}
<style>
  .layui-form-label {
      width: auto !important;
  }
  .layui-table-cell {
      height: auto;
      line-height: 28px;
  }
</style>

<!-- Main content -->
<section class="content">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h3 class="panel-title">基本信息</h3>
    </div>

  <div class="row">
    <div class="col-xs-12">
      <!-- /.box -->
      <div class="box">
        <blockquote class="layui-elem-quote">
          <!-- <button class="layui-btn layui-btn-sm" onclick="add()">新增部门</button> -->
          项目清单
        </blockquote>

        <!-- /.box-header -->
        <div class="box-body">
          <!-- 表格 -->
          <table class="layui-hide" id="table_id" lay-filter="table_id"></table>

          <script type="text/html" id="titleTpl">
            
              <div class="layui-input-inline">
                <select name="xmname" lay-verify="required" lay-search="" lay-filter="testSelect" data-value="">
                  <option value="">直接选择或搜索选择</option>
                  <option value="1">layer</option>
                  <option value="2">form</option>
                  <option value="3">layim</option>
                  <option value="4">element</option>
                  <option value="5">laytpl</option>
                  <option value="6">upload</option>
                  <option value="7">laydate</option>
                  <option value="8">laypage</option>
                </select>
              </div>
          </script>
          
        </div>
        <!-- /.box-body -->
      </div>
      <!-- /.box -->
    </div>
    <!-- /.col -->
  </div>
  <!-- /.row -->

</section>
<!-- /.content -->
{/block}

<!-- 引入js 必须要这两个 2018年1月22日10:06:35-->
{block name='script'}
<script type="text/javascript">

  layui.use(['form', 'laydate'], function(){
    var form = layui.form,
    laydate = layui.laydate;
    //日期
    laydate.render({
      elem: '#date1'
    });
  })

  //项目列表
  let plist = [
                { id:1,name: 'ss',unit:'',price:'0.00',quantity:'0.00',total:'0.00'},
                { id:1,name: '',unit:'',price:'0.00',quantity:'0.00',total:'0.00'},
                { id:1,name: '',unit:'',price:'0.00',quantity:'0.00',total:'0.00'},
            ];

  initTable();
  //表格数据初始化渲染
  function initTable(name) {
    layui.use(['table', 'form'], function () {
      let table = layui.table;
      let form = layui.form;
      //初始化表格
      table.render({
        elem: '#table_id',
        data: plist,
        done: function (res, curr, count) {
          console.log("参数",res,curr,count)
            layui.each($('select'), function (index, item) {
                var elem = $(item);
                elem.val(elem.data('value')).parents('div.layui-table-cell').css('overflow', 'visible');
            });
            table.render();
        },
        cols: [
          [
            {
                field: 'name',
                title: '名称',
                align: 'center',
                width: 200,
                templet: '#titleTpl'
            },
            {
              field: 'unit',
              minwidth: 50,
              title: '单位',
              edit: 'text'
            },
            {
              field: 'price',
              minwidth: 50,
              title: '单价',
              edit: 'text'
            },
            {
              field: 'quantity',
              minwidth: 50,
              title: '工程量',
              edit: 'text'
            },
            {
              field: 'total',
              minwidth: 50,
              title: '合计',
              edit: 'text',
              id:'hhh',
              name:'hhh'
            },
            
          ]
        ]
      });

      // 监听修改update到表格中
      form.on('select(testSelect)', function (data) {
        // console.log("下拉数据",data)
        var elem = $(data.elem);
        var trElem = elem.parents('tr');
        var idx = trElem.data('index');
        plist[idx].id=data.value; //给表格数据赋值

        console.log("行下标",idx,plist)

      });

      //行内编辑
      table.on('edit(table_id)', function(obj){ //注：edit是固定事件名，test是table原始容器的属性 lay-filter="对应的值"
        console.log(obj.value); //得到修改后的值
        console.log(obj.field); //当前编辑的字段名
        console.log(obj.data); //所在行的所有相关数据  

        console.log("表格的所有数据："+JSON.stringify(layui.table.cache.table_id)); 
        plist = layui.table.cache.table_id; 
        console.log("表数据常量：",plist);  
      });
    });
  }

</script>
{/block}
```