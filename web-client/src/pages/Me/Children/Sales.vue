<template>
  <div id="sales">
    <el-table :data="orderList" style="width: 100%">
      <el-table-column label="商品主图">
        <template slot-scope="scope">
          <el-image
            style="width: 100px; height: 100px"
            :src="scope.row.image_url"
            fit="cover"
          ></el-image>
        </template>
      </el-table-column>
      <el-table-column prop="goods_name" label="商品名称"> </el-table-column>
      <el-table-column prop="price" label="单价"> </el-table-column>
      <el-table-column prop="create_at" label="下单时间"> </el-table-column>
      <el-table-column label="状态">
        <template slot-scope="scope">
          <span>{{ filterOrderStatus(scope.row.order_status) }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
            type="primary"
            v-if="scope.row.order_status == 1"
            @click="updateStatus(scope.row.id, 2)"
            >确认收货</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { mapState } from "vuex"
import { mapActions } from "vuex"
import { getOrderList, updateOrderStatus } from "@/api/index"
import { filterOrderStatus } from "@/config/utils"

export default {
  computed: {
    ...mapState(["userInfo"])
  },
  data() {
    return {
      orderList: []
    }
  },
  mounted() {
    this.getList()
  },
  methods: {
    ...mapActions(["logOut"]),
    filterOrderStatus: filterOrderStatus,
    async getList() {
      const res = await getOrderList({
        user_id: this.userInfo.id,
        status: 0
      })
      this.orderList = res.message
    },
    // 修改订单状态
    updateStatus(id, status) {
      updateOrderStatus({
        id: id,
        status: status
      }).then(res => {
        this.$message({
          message: res.message,
          type: "success"
        })
        this.getList()
      })
    }
  }
}
</script>

<style scoped></style>
