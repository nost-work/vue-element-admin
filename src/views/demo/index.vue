<template>
  <div class="app-container">
    <h1>MicroCMS検証用テスト</h1>
    <el-table ref="dragTable" v-loading="listLoading" :data="items" border fit highlight-current-row style="width: 100%">

      <el-table-column width="180px" label="タイトル">
        <template slot-scope="{row}">
          <span>{{ row.title }}</span>
        </template>
      </el-table-column>

      <el-table-column min-width="300px" label="本文">
        <template slot-scope="{row}">
          <span>{{ row.body }}</span>
        </template>
      </el-table-column>

    </el-table>

  </div>
</template>

<script>
import axios from 'axios'
// import { mapGetters } from 'vuex'
const headers = {
  'X-API-KEY': '278b7a01-4ef1-498c-8892-f4705425ec30'
}
export default {
  name: 'Demo',
  components: {},
  data() {
    return {
      items: []
    }
  },
  computed: {},
  mounted() {
    axios
      .get('https://nost.microcms.io/api/v1/test', {
        headers: headers,
        data: {}
      })
      .then(res => {
        this.items = res.data.contents
      })
  }
}
</script>
