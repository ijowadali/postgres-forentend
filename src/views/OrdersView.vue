<template>
  <div class="app-container">
    <div style="padding: 20px 20px" class="filter-container">
      <el-row :gutter="20">
        <el-col :lg="5" class="card-panel-col">
          <el-input v-model="pageFilters.name" placeholder="Search by name" />
        </el-col>
        <el-col :lg="6" class="card-panel-col">
          <el-date-picker
            v-model="pageFilters.date"
            type="daterange"
            start-placeholder="Start date"
            end-placeholder="End date"
          />
        </el-col>
        <el-col :lg="2" class="card-panel-col">
          <el-button icon="el-icon-search" type="primary"> Find </el-button>
        </el-col>
      </el-row>
    </div>
    <el-table :data="list" border style="width: 100%">
      <el-table-column prop="customer_id" label="customer id" width="180" />
      <el-table-column prop="order_name" label="order" width="180" />
      <el-table-column prop="CreatedAt" label="CreatedAt" />
    </el-table>
    <el-pagination
      v-model:currentPage="listQuery.page"
      :page-size="listQuery.size"
      small
      background
      layout="total, prev, pager, next"
      :total="listQuery.total"
      @current-change="handleCurrentChange"
      @pagination="getList(false)"
    />
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      pageFilters: {
        status: [],
      },
      total: 0,
      list: [],
      listQuery: {
        total: 0,
        page: 0,
        size: 5,
        sortDirection: "",
        sortColumn: "",
      },
    };
  },
  mounted() {
    this.getList(true);
  },
  methods: {
    handleCurrentChange(val) {
      this.listQuery.page = val - 1;
      this.getList(false);
    },
    async getList(start = false) {
      const res = await axios.get("http://127.0.0.1:8082/orders", {
        params: { ...this.listQuery },
      });
      this.list = res.data.items;
      // this.list = res.data;
      this.listQuery.total = res.data.total;
      if (start) {
        this.listQuery.page = 1;
      } else {
        this.listQuery.page = res.data.page + 1;
      }
      // this.listLoading = false;
    },
  },
};
</script>
