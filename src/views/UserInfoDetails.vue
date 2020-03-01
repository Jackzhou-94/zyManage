<template>
  <a-form :form="form" @submit="handleSubmit">
    <a-form-item v-bind="formItemLayout" label="人员ID">
      <a-input
        disabled
        v-decorator="[
          'user_Id',
          {
              initialValue:userInfoData.user_id
          }
        ]"
      />
    </a-form-item>

    <a-form-item v-bind="formItemLayout" label="姓名">
      <a-input
        v-decorator="[
          'userName',
          {
            initialValue:userInfoData.username,
            rules: [{
              required: true, message: '姓名不允许为空',
            }]
          }
        ]"
      />
    </a-form-item>

    <a-form-item v-bind="formItemLayout" label="E-mail">
      <a-input
        v-decorator="[
          'email',
          {
              initialValue:userInfoData.email,
            rules: [{
              type: 'email', message: '输入的电子邮件无效!',
            }, {
              required: true, message: '请输入 E-mail!',
            }]
          }
        ]"
      />
    </a-form-item>

    <a-form-item v-bind="formItemLayout" label="年龄">
      <a-input
        v-decorator="[
          'age',
          {
              initialValue:userInfoData.age,
          }
        ]"
      />
    </a-form-item>

    <a-form-item v-bind="formItemLayout" label="手机号">
      <a-input
        v-decorator="[
          'mobile',
          {
              initialValue:userInfoData.mobile,
          }
        ]"
      />
    </a-form-item>
    <a-form-item v-bind="tailFormItemLayout">
      <a-button type="primary" html-type="submit">确认</a-button>
      <a-button @click="close" class="close-btn">关闭</a-button>
    </a-form-item>
  </a-form>
</template>

<script>
const residences = [
  {
    value: "zhejiang",
    label: "Zhejiang",
    children: [
      {
        value: "hangzhou",
        label: "Hangzhou",
        children: [
          {
            value: "xihu",
            label: "West Lake"
          }
        ]
      }
    ]
  },
  {
    value: "jiangsu",
    label: "Jiangsu",
    children: [
      {
        value: "nanjing",
        label: "Nanjing",
        children: [
          {
            value: "zhonghuamen",
            label: "Zhong Hua Men"
          }
        ]
      }
    ]
  }
];

export default {
  data() {
    return {
      confirmDirty: false,
      residences,
      autoCompleteResult: [],
      formItemLayout: {
        labelCol: {
          xs: { span: 24 },
          sm: { span: 8 }
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 }
        }
      },
      tailFormItemLayout: {
        wrapperCol: {
          xs: {
            span: 24,
            offset: 0
          },
          sm: {
            span: 16,
            offset: 8
          }
        }
      },
      userInfoData: {}
    };
  },
  beforeCreate() {
    this.form = this.$form.createForm(this);
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFieldsAndScroll((err, values) => {
        if (!err) {
          this.$message.success("修改成功");
          this.$router.go(-1);
        }
      });
    },
    close() {
      this.$router.go(-1);
    },
    getUserInfo() {
      this.$axios({
        method: "get",
        url: "http://localhost:8080/userInfoDetails.json"
      }).then(res => {
        let data = res.data;
        let user = data.filter(item => {
          return item.user_id == this.$route.params.id;
        });
        this.userInfoData = user[0];
      });
    },
    handleConfirmBlur(e) {
      const value = e.target.value;
      this.confirmDirty = this.confirmDirty || !!value;
    },
    compareToFirstPassword(rule, value, callback) {
      const form = this.form;
      if (value && value !== form.getFieldValue("password")) {
        callback("Two passwords that you enter is inconsistent!");
      } else {
        callback();
      }
    },
    validateToNextPassword(rule, value, callback) {
      const form = this.form;
      if (value && this.confirmDirty) {
        form.validateFields(["confirm"], { force: true });
      }
      callback();
    },
    handleWebsiteChange(value) {
      let autoCompleteResult;
      if (!value) {
        autoCompleteResult = [];
      } else {
        autoCompleteResult = [".com", ".org", ".net"].map(
          domain => `${value}${domain}`
        );
      }
      this.autoCompleteResult = autoCompleteResult;
    }
  },
  created() {
    this.getUserInfo();
  }
};
</script>
<style scoped>
.close-btn {
  margin-left: 20px;
}
</style>