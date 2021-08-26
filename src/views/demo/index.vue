<template>
  <div class="app-container">
    <h1>MicroCMS検証用テスト</h1>
    <el-table
      v-loading="listLoading"
      :data="list"
      border
      fit
      highlight-current-row
      style="width: 100%"
    >
      <el-table-column align="center" label="ID" width="80">
        <template slot-scope="{ row }">
          <span>{{ row.rowId }}</span>
        </template>
      </el-table-column>

      <el-table-column min-width="300px" label="タイトル">
        <template slot-scope="{ row }">
          <template v-if="row.edit">
            <el-input v-model="row.title" class="edit-input" size="small" />
          </template>
          <span v-else>{{ row.title }}</span>
        </template>
      </el-table-column>
      <el-table-column min-width="300px" label="本文">
        <template slot-scope="{ row }">
          <template v-if="row.edit">
            <el-input v-model="row.body" class="edit-input" size="small" />
          </template>
          <span v-else>{{ row.body }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" label="アクション" width="250">
        <template slot-scope="{ row }">
          <el-button
            v-if="row.edit"
            type="success"
            size="small"
            icon="el-icon-circle-check-outline"
            @click="confirmEdit(row)"
          >
            保存
          </el-button>
          <el-button
            v-if="row.edit"
            class="cancel-btn"
            size="small"
            icon="el-icon-refresh"
            type="warning"
            @click="cancelEdit(row)"
          >
            キャンセル
          </el-button>
          <div v-else class="">
            <el-button
              type="primary"
              size="small"
              icon="el-icon-edit"
              @click="row.edit = !row.edit"
            >
              編集
            </el-button>
            <el-button
              type="danger"
              size="small"
              icon="el-icon-delete"
              @click="deleteData(row)"
            >
              削除
            </el-button>
          </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
// import { mapGetters } from 'vuex'
const apiUrl = 'https://nost.microcms.io/api/v1/test'
const getHeaders = {
  'X-API-KEY': '278b7a01-4ef1-498c-8892-f4705425ec30'
}
const postHeaders = {
  'Content-Type': 'application/json',
  'X-WRITE-API-KEY': '15c6b94e-f2d1-4c93-8dba-121bc3649d48'
}
export default {
  name: 'Demo',
  components: {},
  data() {
    return {
      listLoading: true,
      list: null
    }
  },
  computed: {},
  mounted() {
    axios
      .get(apiUrl, {
        headers: getHeaders,
        data: {}
      })
      .then(res => {
        this.list = res.data.contents.map((data, index) => {
          const output = {
            id: data.id,
            rowId: index + 1,
            title: data.title,
            body: data.body,
            original: {
              title: data.title,
              body: data.body
            },
            edit: false
          }
          return output
        })
        this.listLoading = false
      })
  },
  methods: {
    cancelEdit(row) {
      row.title = row.original.title
      row.body = row.original.body
      row.edit = false
      this.$message({
        message: 'キャンセルしました。',
        type: 'warning'
      })
    },
    confirmEdit(row) {
      const data = {
        title: row.title,
        body: row.body
      }
      row.edit = false
      axios
        .patch(`${apiUrl}/${row.id}`, data, {
          headers: postHeaders
        })
        .then(res => {
          console.log(res)
        })
      this.$message({
        message: '編集しました。',
        type: 'success'
      })
    },
    deleteData(row) {
      axios
        .delete(`${apiUrl}/${row.id}`, {
          headers: postHeaders
        })
        .then(res => {
          console.log(res)
        })
      this.$message({
        message: '削除しました。',
        type: 'danger'
      })
    }
  }
}
</script>
