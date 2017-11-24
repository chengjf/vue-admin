<template>
  <div class="menu-right" v-if="get_user_info.login">
    <div class="notification-menu">
      <el-dropdown trigger="click" class="notification-list">
        <div class="notification-btn">
          <img :src="get_user_info.user.avatar" :alt="get_user_info.user.name"/>
          <span v-text="get_user_info.user.name"></span>
          <span class="icon"></span>
        </div>
        <el-dropdown-menu slot="dropdown" class="dropdown-menu">
          <el-dropdown-item class="dropdown-list">
            <a href="javascript:" class="dropdown-btn" @click="user_click(1)">
              <i class="icon fa fa-user"></i>
              <span>个人信息</span>
            </a>
          </el-dropdown-item>
          <el-dropdown-item class="dropdown-list">
            <a href="javascript:" class="dropdown-btn" @click="user_click(2)">
              <i class="icon fa fa-cog"></i>
              <span>设置</span>
            </a>
          </el-dropdown-item>
          <el-dropdown-item class="dropdown-list">
            <a href="javascript:" class="dropdown-btn" @click="user_click(0)">
              <i class="icon fa fa-sign-out"></i>
              <span>安全退出</span>
            </a>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
    <div>
      <el-dialog title="get_user_info.user.name" :visible.sync="dialogFormVisible" width="30%"
         :modal-append-to-body="false">
        <span v-text="get_user_info.user.name"></span>
        <el-form >
          <el-form-item label="姓名">
            <el-input v-model="user.name" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="年龄">
            <el-input v-model="user.age" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="头像">
            <el-input v-model="user.avatar" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="描述">
            <el-input v-model="user.desc" auto-complete="off"></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">取 消</el-button>
          <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
        </div>
      </el-dialog>
    </div>
  </div>
</template>
<script type="text/javascript">
  import {mapGetters, mapActions} from 'vuex'
  import {GET_USER_INFO} from 'store/getters/type'
  import {SET_USER_INFO} from 'store/actions/type'

  const USER_OUT = 0
  const USER_INFO = 1
  const USER_SETTING = 2

  export default{
    data: function(){
      return {
        dialogFormVisible: false,
        user: {
          name: "Jeff Cheng",
          age: "18",
          avatar: "https:\/\/avatars0.githubusercontent.com/u/16893554?v=3&s=240",
          desc: "管理员就是我"
        }
      }
    },
    created: function(){
      this.user = this.get_user_info
    },
    computed: {
      ...mapGetters({
        get_user_info: GET_USER_INFO
      })
    },
    methods: {
      ...mapActions({
        set_user_info: SET_USER_INFO
      }),
      //退出
      user_out(){
        this.$confirm('此操作将退出登录, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$fetch.api_user.logout()
            .then(({msg}) => {
              this.$message.success(msg)
              this.set_user_info(null)
              setTimeout(this.$router.replace({name: "login"}), 500)
            })
        }).catch(() => {

        })
      },
      user_info() {
        //个人信息
        this.user = this.get_user_info

        this.dialogFormVisible = true

        // var name = this.get_user_info.user.name
        // this.$alert('姓名：' + name, '个人信息', {
        //   dangerouslyUseHTMLString: true
        // });
      },
      user_setting() {
        //设置
        var name = this.get_user_info.user.name
         this.$alert('姓名：' + name, '设置', {
          dangerouslyUseHTMLString: true
        });
      },
      user_click(type) {
        switch (type) {
          case USER_OUT :
            //退出
            this.user_out()
            break
          case USER_INFO:
            //个人信息
            this.user_info()
            break
          case USER_SETTING:
            //设置
            this.user_setting()
            break
        }
      }
    }
  }
</script>
