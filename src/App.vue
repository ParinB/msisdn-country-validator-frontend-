<template>
  <div id="app" class="container">
    <h2 class="text-center">Customers</h2>
    <el-table
      border
      :data="
 pagedTableData.filter(
          (data) =>
            !search
            || data.country.toLowerCase().includes(search.toLowerCase())
            || data.state.toLowerCase().includes(search.toLowerCase())
        )
      "
      style="width: 100%"
    >
      <el-table-column label="Customer Name" prop="name"> </el-table-column>
      <el-table-column label="Msisdn" prop="msisdn"> </el-table-column>
      <el-table-column label="Country" prop="country"> </el-table-column>
      <el-table-column label="Status" prop="state"> </el-table-column>
      <el-table-column align="right">
        <template slot="header" slot-scope="scope">
          <el-input v-model="search" size="mini" placeholder="Type to search" />
        </template>
        <template slot-scope="scope">{{ scope.row.country_code }}</template>
      </el-table-column>
    </el-table>
    <div class="paginate">
      <el-pagination
        @current-change="handleCurrentChange"
        :current-page="currentPage"
        :page-size="pageSize"
        background
        layout="prev, pager, next"
        :total="customers.length"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",

  data() {
    return {
      search: "",
      pageSize: 10,
      currentPage: 1,
      customers : "",
    };
  },
  computed: {
    pagedTableData() {
      return this.customers.slice(
          this.pageSize * this.currentPage - this.pageSize,
          this.pageSize * this.currentPage
      );
    },
  },
  methods: {
    handleCurrentChange(val) {
      this.currentPage = val;
    },
    fetch_customers: function () {
      axios.get(`http://127.0.0.1:5555/customers`,)
          .then((res) => {
            this.customers = res.data
            console.log(this.customers)
          })
          .catch((error) => {
            this.errors = error
          })
    }
  },
  created() {
    this.fetch_customers()
  },
};
</script>

<style lang="scss">
.paginate {
  margin-top: 20px;
}
.container {
  padding: 10px;
  width: 80%;
  margin: 0 auto;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
