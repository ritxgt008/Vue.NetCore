<template>
  <div>
    <div class="i-text">
      <h2>组件引入方式</h2>
    </div>
    <pre class="bg-ms">
        <code>
         import VolForm from "@/components/basic/VolForm.vue";
        </code>
      </pre>
    <div class="i-text">
      <h2>表单</h2>
    </div>
    <VolForm
      ref="myform"
      :label-width="150"
      :loadKey="true"
      :formFileds="formFileds1"
      :formRules="formRules1"
    ></VolForm>
    <div>
      <Button type="info" long @click="reset">重置表单</Button>
    </div>
    <div>
      <docParamTable name="form"></docParamTable>
    </div>
  </div>
</template>
<script>
import VolForm from "@/components/basic/VolForm.vue";
import docParamTable from "./doc_ParamTable.vue";
export default {
  components: { VolForm, docParamTable },
  methods: {
    reset() {
      this.$refs.myform.reset();
      this.$Message.error("表单已重置");
    }
  },
  data() {
    return {
      formFileds1: {
        Variety: 1,
        AgeRange: "",
        DateRange: [],
        City: "北京市",
        AvgPrice: 8.88,
        number1: 20,
        mail: "",
        Date: "",
        IsTop: "",
        Fruits: [],
        Other: "",
        extra1: "添加其他标签",
        extra2: "添加其他标签可点击",
        Switch: 1,
        SelectList: ["北京市", "上海市", "天津市", "广州市", "重庆市"],
        remoteSearch: "",
        readonlyText: "还没想好....",
        readonlyImg:
          "https://imgs-1256993465.cos.ap-chengdu.myqcloud.com/h5pic/x2.jpg",
        ProImg:
          "https://imgs-1256993465.cos.ap-chengdu.myqcloud.com/h5pic/x3.jpg"
        //多个图片可以用逗号隔开
      },
      formRules1: [
        //两列的表单，formRules数据格式为:[[{},{}]]
        [
          {
            dataKey: "city",
            title: "自动绑定数据源",
            required: true,
            field: "City",
            data: [],
            type: "select"
          },
          {
            title: "手动绑定数据源",
            dataKey: "age",
            placeholder: "在这里可设置提示描述",
            //如果这里绑定了data数据，后台不会加载此数据源
            data: [{ key: 1, value: "是" }, { key: 0, value: "否" }],
            required: false,
            field: "Variety",
            type: "select"
          }
        ],
        [
          {
            title: "手机号",
            required: true, //设置为必选项
            field: "AgeRange",
            type: "phone",
            onKeyPress: $event => {
              if ($event.keyCode == 13) {
                this.$Message.error(this.formFileds1.AgeRange + "");
              }
            }
          },
          {
            title: "date日期",
            field: "Date",
            type: "datetime"
          }
        ],
        [
          {
            title: "多选日期",
            range: true, //设置为true可以选择开始与结束日期
            required: false,
            field: "DateRange",
            type: "date"
          },
          {
            type: "number",
            title: "数字",
            required: true,
            placeholder: "你可以自己定义placeholder显示的文字",
            field: "number1"
          }
        ],
        [
          {
            type: "decimal",
            title: "最大最小decimal值",
            max: 10,
            min: 2,
            required: true,
            field: "AvgPrice"
          },
          {
            title: "邮箱",
            field: "mail",
            range: true, //设置为true可以选择开始与结束日期
            required: false,
            type: "mail"
          }
        ],
        [
          {
            title: "额外标签",
            required: true,
            type: "text",
            field: "extra1",
            extra: {
              icon: "ios-search",
              style: "color:red",
              text: "这里可以显示额外标签"
            }
          },
          {
            title: "可点击额外标签",
            field: "extra2",
            type: "text",
            extra: {
              icon: "ios-search", //显示图标
              text: "点击可触发事件", //显示文本
              //触发事件
              click: item => {
                this.$Message.error("点击标签触发的事件");
              }
            }
          }
        ],
        [
          {
            dataKey: "city",
            title: "多选",
            required: true,
            field: "SelectList",
            data: [],
            type: "selectList"
          },
          {
            dataKey: "city",
            title: "远程搜索",
            required: true,
            field: "remoteSearch",
            remote: true, //远程搜索(从后台字典搜索,只会搜索自定义sql,后台如果返回的数据量较大，请将字典维护的sql取top)
            //url:'api/xx/xx'从指定url搜索 返回格式必须是[{key:'xxxx',value:'xxx'}]
            data: [],
            type: "select"
          }
        ],
        [
          {
            title: "自定义验证",
            required: true,
            field: "Other",
            validator: (rule, val) => {
              if (val != "234") {
                return "必须输入【234】";
              }
              return "";
            }
          },
          {
            title: "Switch",
            field: "Switch",
            dataKey: "enable", //这里会从后台自动绑定数据源
            data: [],
            required: false,
            type: "switch"
          }
        ],
        [
          //readonlyImg
          {
            title: "checkbox",
            //如果这里绑定了data数据，后台不会加载此数据源
            data: [
              { key: 0, value: "冬瓜" },
              { key: 1, value: "西瓜" },
              { key: 2, value: "南瓜" },
              { key: 3, value: "哈密瓜" }
            ],
            field: "Fruits",
            type: "checkbox"
          },
          {
            title: "字段只读",
            readonly: true, //设置readonly或disabled都行
            field: "readonlyText",
            type: "text"
          }
        ],
        [
          {
            title: "备注",
            required: true,
            field: "IsTop",
            min: 3,
            max: 5,
            placeholder: "至少输入3个字符,最多只能输入5个字符",
            colSize: 12, //设置12，此列占100%宽度
            type: "textarea"
          }
        ],
        [
          {
            title: "图片只读",
            readonly: true, //设置readonly或disabled都行
            field: "readonlyImg",
            type: "img"
          },
          {
            title: "可上传图片",
            field: "ProImg",
            type: "img",
            maxSize: 0.2, //最大0.2M图片
            //上传参数可参照volupload组件
            url: "/api/App_Appointment/Upload"
          }
        ]
      ]
    };
  }
};
</script>