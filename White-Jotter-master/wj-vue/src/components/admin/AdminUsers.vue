<template>
  <el-container>
    <div>
      <el-row style="z-index: 1;">
        <a href="/index">
          <img src="../../assets/img/icon/icon2.png" alt="" width="80px" style="float: left;margin-top: -5px;">
        </a>
        <span style="font-size: 32px;font-weight: bold;position:absolute;left: 100px">学霸笔记</span>
        <i class="el-icon-s-unfold" style="font-size: 20px;float: right"></i>
      </el-row>
      <el-row style="z-index: 0;">
        <el-col style="width: 250px" >
          <admin-menu></admin-menu>
        </el-col>
      </el-row>
    </div>
    <el-table
      fixed="right"
      :data="users"
      border
      style
    >
      <el-table-column
        fixed
        prop="id"
        label="ID"
        width="300"
      ></el-table-column>
      <el-table-column
        prop="username"
        label="username"
        width="350">
      </el-table-column>
      <el-table-column
        prop="data"
        label="姓名"
        width="350">
      </el-table-column>
      <el-table-column
        label="操作"
        width="100">
        <template slot-scope="scope">
          <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
          <el-button @click="deleteUser(scope.row.id)" type="text" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-container>
</template>

<script>

    import AdminMenu from './AdminMenu'
    export default {
        name: 'Users',
        components: {AdminMenu},
      mounted: function () {
        this.loadUsers()
      },

        data () {
            return {
              users: []
            }
        },

      methods: {
        loadUsers () {
          var _this = this
          this.$axios.get('/users').then(resp => {
            if (resp && resp.status === 200) {
              _this.users = resp.data
            }
          })
        },
        deleteUser (id) {
          this.$confirm('此操作将永久删除该书籍, 是否继续?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
              this.$axios
                .post('/user/delete1', {id: id}).then(resp => {
                if (resp && resp.status === 200) {
                  this.loadUsers()
                }
              })
            }
          ).catch(() => {
            this.$message({
              type: 'info',
              message: '已取消删除'
            })
          })
        }
      }
    }
</script>
