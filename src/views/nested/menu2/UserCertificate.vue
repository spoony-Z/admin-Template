<template>
  <div class="userCertific">
    <el-card shadow="never">
      <el-form inline size="small" :model="formInline" class="demo-form-inline">
        <el-form-item label="城市:">
          <el-select v-model="formInline.userName" placeholder="请选择用户">
            <el-option v-for="v in crty" :key="v.userName" :label="v.userName" :value="v.userName" />
          </el-select>
        </el-form-item>
        <el-form-item label="时长:">
          <el-date-picker
            v-model="formInline.time"
            type="daterange"
            unlink-panels
            format="yyyy-MM-dd HH:mm:ss"
            value-format="yyyy-MM-dd+HH:mm:ss"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
          />
        </el-form-item>
        <el-form-item label="接口:">
          <el-select v-model="formInline.interfaceName" placeholder="请选择接口">
            <el-option v-for="v in interfaces" :key="v.interfaceName" :label="v.interfaceName" :value="v.interfaceName" />
          </el-select>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="onSubmit">查询</el-button>
        </el-form-item>
      </el-form>
    </el-card>
    <el-card style="margin-top: 1rem" shadow="never">
      <Charts height="600px" width="100%" />
    </el-card>
  </div>
</template>

<script>
import Charts from './UserCertificateEcharts/index'

export default {
  components: { Charts },
  data() {
    return {
      formInline: {
        userName: '',
        interfaceName: '',
        time: []
      },
      xAxis: [],
      yAxis: [],
      crty: [],
      interfaces: []
    }
  },
  watch: {
    'formInline.userName': {
      handler(value) {
        if (value) {
          this.formInline.interfaceName = ''
          this.getInterface(value)
        }
      }
    }
  },
  created() {
    this.getCrty()
    this.getChart()
  },
  methods: {
    getChart(formInline = { }) {
      this.$api.get(`/mgr/WeBASE-Node-Manager/monitor/transList/1`, formInline, ({ data }) => {
        this.xAxis = data.data.transInfoList.map(el => {
        })
      })
    },
    getCrty() {
      this.$api.get(`/mgr/WeBASE-Node-Manager/monitor/userList/1`, {}, ({ data }) => {
        this.crty = data.data
      })
    },
    getInterface(userName) {
      this.$api.get(`/mgr/WeBASE-Node-Manager/monitor/interfaceList/1?userName=${userName}`, {}, ({ data }) => {
        this.interfaces = data.data
      })
    },
    onSubmit() {
      const { formInline } = this
      formInline['startDate'] = formInline.time[0]
      formInline['endDate'] = formInline.time[1]
      delete formInline.time
      this.getChart(formInline)
    }
  }
}
</script>

<style scoped>
.chart-container2 {
  position: relative;
  width: 100%;
  height: calc(100vh);
}
.userCertific {
  padding: 1.25rem;
}
</style>
