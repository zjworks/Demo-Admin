<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avater_box">
        <img src="../assets/logo.png" alt="logo" />
      </div>
      <!-- 登录表单 -->
      <el-form ref="loginFormRef" status-icon :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
          <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input prefix-icon="iconfont icon-user" placeholder="请输入用户名" v-model="loginForm.username"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input type="password" prefix-icon="iconfont icon-3702mima" placeholder="请输入密码" v-model="loginForm.password"></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      //   输入校验
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'change' }
        ]
      }
    }
  },
  mounted () {
    this.loginForm.username = 'admin'
    this.loginForm.password = '123456'
  },
  methods: {
    // 重置按钮
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    // 登录按钮
    login () {
      // 进行预验证
      this.$refs.loginFormRef.validate(async vali => {
        if (!vali) return
        // 发送请求，对象结构出data
        const { data: result } = await this.$http.post('login', this.loginForm)
        // 判断服务器返回的状态码
        if (result.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        // 登录成功够将token 保存到sessionStorage中
        window.sessionStorage.setItem('token', result.data.token)
        // 跳转页面
        this.$router.push('/home')
      })
    }
  }

}
</script>

<style lang="less" scoped>
.login_container {
  height: 100%;
  background-color: #2b4b6b;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 5px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);

    .avater_box {
        height: 130px;
        width: 130px;
        border: 4px solid #eee;
        border-radius: 50%;
        overflow: hidden;
        position: absolute;
        box-shadow: 0 0 4px #000;
        background-color: #fff;
        left: 50%;
        transform: translate(-50%, -50%);
        img {
        width: 100%;
        height: 100%;
        }
    }

    .login_form {
        box-sizing: border-box;
        position: absolute;
        top: 110px;
        width: 100%;
        padding: 0 22px;
    }

    .btns {
        float: right;
    }
}
</style>
