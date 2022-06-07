<template>
  <div class="chart-container">
    <el-card shadow="never">
      <el-form size="small" inline :model="query">
        <el-form-item label="起止时间" prop="time" style="width: 100%">
          <el-date-picker
            v-model="query.time"
            type="daterange"
            unlink-panels
            format="yyyy-MM-dd HH:mm:ss"
            value-format="yyyy-MM-dd[T]HH:mm:ss"
            range-separator="至"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
          />
        </el-form-item>
        <el-form-item label="数据粒度" prop="granularit" style="width: 28.6%">
          <el-radio-group v-model="query.granularit">
            <el-radio label="1">5分钟</el-radio>
            <el-radio label="2">1分钟</el-radio>
            <el-radio label="3">5秒钟</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item>
          <el-button type="primary">查询</el-button>
        </el-form-item>
      </el-form>
    </el-card>
    <div
      style="
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
      "
    >
      <chart text="区块高度" height="300px" width="49%" />
      <chart text="pbftView" height="300px" width="49%" />
      <chart text="带打包的存证数" height="300px" width="49%" />
      <chart text="平均快大小" height="300px" width="49%" />
      <chart text="平均出块周期" height="300px" width="49%" />
      <chart text="平均TPS" height="300px" width="49%" />
    </div>
  </div>
</template>

<script>
import Chart from "@/components/Charts/LineMarker";

export default {
  name: "LineChart",
  components: { Chart },
  data() {
    return {
      query: {
        time: [],
        granularit: "1",
      },
    };
  },

  watch: {
    "query.time": {
      handler(time) {
        console.log(time);
      },
    },
  },
  methods: {
    MonitoringList() {
      this.$api.getMonitor(
        "/mgr/WeBASE-Node-Manager/stat?gap=1&beginDate=1652766512957&endDate=1652768905252&groupId=1",
        {},
        (res) => {
          console.log("*********",res);   
        }
      );
    },
  },

  created() {
    this.MonitoringList();
  },
};
</script>

<style scoped>
.chart-container {
  padding: 20px;
  /* position: relative;
  width: 100%;
  height: calc(100vh - 84px); */
}
</style>

