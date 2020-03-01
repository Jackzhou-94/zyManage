<template>
  <a-table
    :columns="columns"
    rowKey="user_id"
    :dataSource="userList"
    :pagination="pagination"
    :loading="loading"
  >
    <template slot="operation" slot-scope="text, record">
      <a-button slot="operation" type="link" @click="Jump(record.user_id)">查看详情</a-button>
    </template>
  </a-table>
</template>
<script>
const columns = [
  {
    title: "人员Id",
    dataIndex: "user_id",
    width: "20%"
  },
  {
    title: "姓名",
    dataIndex: "username",
    width: "20%"
  },
  {
    title: "年龄",
    dataIndex: "age"
  },
  {
    title: "操作",
    scopedSlots: { customRender: "operation" }
  }
];

export default {
  mounted() {},
  data() {
    return {
      userList: [],
      pagination: {},
      loading: false,
      columns
    };
  },
  methods: {
    Jump(userid) {
      console.log(userid);
      this.$router.push(`UserInfoDetails/${userid}`)
    },
    getUserInfo() {
      this.$axios({
        method: "get",
        url: "http://localhost:8080/userList.json"
      }).then(res => {
        this.userList = res.data;
      });
    }
  },
  created() {
    this.getUserInfo();
  }
};
</script>
