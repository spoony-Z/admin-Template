<template>
  <div class="deposit_info">
    <el-card>
      <el-row>
        <el-col :span="10">
          <el-form inline :model="deposit_info_search">
            <el-form-item>
              <el-input v-model="deposit_info_search.blockNumber" />
            </el-form-item>
            <el-form-item>
              <el-button>查询</el-button>
            </el-form-item>
          </el-form>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-table :data="tableData">
            <el-table-column type="expand">
              <template slot-scope="scope">
                <el-card>
                  <div style="display: flex; height: 40px">
                    <div
                      class="line"
                      style="
                        width: 5px;
                        height: 20px;
                        background-color: #188efb;
                      "
                    />
                    <div
                      style="
                        color: #188efb;
                        font-size: 18px;
                        margin-left: 0.2rem;
                      "
                    >
                      存证信息
                    </div>
                  </div>
                  <el-el-form title="存证信息">
                    <el-el-form-item label="区块哈希">{{
                      scope.row.block_hash
                    }}</el-el-form-item>
                    <el-el-form-item label="区块高度">{{
                      scope.row.block_height
                    }}</el-el-form-item>
                    <el-el-form-item label="发起者地址">{{
                      scope.row.initiator_address
                    }}</el-el-form-item>
                    <el-el-form-item label="合约地址">{{
                      scope.row.contract_address
                    }}</el-el-form-item>
                    <el-el-form-item label="存证哈希">{{
                      scope.row.deposit_hash
                    }}</el-el-form-item>
                    <el-el-form-item label="时间戳">{{
                      scope.row.create_time
                    }}</el-el-form-item>
                    <el-el-form-item label="存证数据" />
                  </el-el-form>
                </el-card>
              </template>
            </el-table-column>
            <el-table-column label="存证哈希" prop="deposit_hash" />
            <el-table-column label="块高" prop="block_height" />
            <el-table-column label="创建时间" prop="create_time" />
          </el-table>
        </el-col>
      </el-row>
      <el-row style="margin-top: 15px">
        <el-col :span="12" :offset="18">
          <el-pagination
            :current-page="page.currentPage"
            :page-sizes="[10, 20, 50]"
            :page-size="page.pageSize"
            layout="total, prev, pager, next, jumper"
            :total="page.totalCount"
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
          />
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'DepositInfo',
  data() {
    return {
      tableData: [],
      deposit_info_search: {
        blockNumber: '',
        transactionHash: ''
      },
      showData: false,
      page: {
        totalCount: 0,
        currentPage: 1,
        pageSize: 10
      }
    }
  },
  watch: {
    'deposit_info_search.blockNumber': {
      handler(value) {
        this.showData = Boolean(value)
        this.deposit_info_search.transactionHash = value
      }
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.$api.get(
        `/mgr/WeBASE-Node-Manager/transaction/transactionReceipt/1/0x1c3f4daea5e067069a556eee3cfa4c07e9f7deab1633e79bd78f3ab6194633ee`,
        {},
        ({ data }) => {
          // this.tableData = data.data
          console.log(data)
          // this.page.totalCount = parseInt(data.totalCount)
        }
      )
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange(val) {
      const { showData, getList, getListSearch } = this
      showData ? getListSearch(val) : getList(val)
      console.log(`当前页: ${val}`)
    },
    getListSearch(currentPage = 1, pageSize = 10) {
      const { blockNumber, transactionHash } = this.deposit_info_search
      this.$api.get(
        `/mgr/WeBASE-Node-Manager/transaction/transList/1/${currentPage}/${pageSize}?blockNumber=${blockNumber}?transactionHash=${transactionHash}`,
        {},
        ({ data }) => {
          this.tableData = data.data
          this.page.totalCount = parseInt(data.totalCount)
        }
      )
    }
  }
}
</script>

<style scoped>
.deposit_info {
  padding: 1rem;
}
</style>
