<template>
  <div class="mycontent">
    <el-button icon="el-icon-d-arrow-left" type="info" plain @click="$router.go(-1)" size="small">返回</el-button>
    <h2>创建api</h2>

    <common ref="apiAdd"></common>

    <el-button @click="save" type="primary" plain>保存</el-button>

  </div>
</template>

<script>
import common from '@/components/api/common'

export default {
  data() {
    return {}
  },
  components: {common},
  methods: {

    save() {
      const detail = this.$refs.apiAdd.detail
      // debugger
      let p = {
        name: detail.name,
        path: detail.path,
        note: detail.note,
        groupId: detail.groupId,
        previlege: detail.previlege,
        cachePlugin: detail.cachePlugin,
        transformPlugin: detail.transformPlugin,
        datasourceId: this.$refs.apiAdd.$refs.sqlCode.datasourceId,
        sql: this.$refs.apiAdd.$refs.sqlCode.codemirror.getValue().trim(),
        params: JSON.stringify(detail.params)
      }

      // console.log(detail)
      if (p.sql == null || p.datasourceId == null || p.name == null
          || p.path == null || p.groupId == null) {
        this.$message.error("必填项未填")
        return
      }

      this.axios.post("/apiConfig/add", p).then((response) => {
        if (response.data.success) {
          this.$message.success(response.data.msg)
        } else {
          this.$message.error(response.data.msg)
        }

      }).catch((error) => {
        this.$message.error("失败")
      })
    }
  },
  created() {

  }
}
</script>

<style scoped>


</style>
