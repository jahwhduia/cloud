<template>
  <div id="login">
    <div class="login_img">
      <div class="login_tit">
        <div></div>
        <div>高血压家庭健康管理</div>
        <div></div>
      </div>
    </div>
    <div class="login_con">
      <div class="login_form">
        <h3>登录</h3>
        <el-form :label-position="labelPosition" :model="form">
          <el-form-item>
            <el-input
              class="phone"
              prefix-icon="el-icon-user-solid"
              v-model="form.phone"
              placeholder="请输入账号/手机号"
            >
            </el-input>
            <el-button class="getnum" round type="primary" @click="postCode">获取验证码</el-button>
            <!-- 2022年4月24日11:07:29注释 -->
          </el-form-item>
          <el-form-item>
            <el-input
              class="password"
              prefix-icon="el-icon-s-goods"
              suffix-icon="el-icon-view"
              type="password"
              v-model="form.password"
              placeholder="请输入密码"
              @keyup.enter.native="handleSubmit('formInline')"
            ></el-input>
         </el-form-item>
          <el-form-item>
            <el-input
              prefix-icon="el-icon-s-goods"
              suffix-icon="el-icon-view"
              v-model="form.code"
              placeholder="请输入获取到的验证码" 
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-select 
             v-model="form.fileOrgType" 
             placeholder="请选择登录角色"
            >
            <!-- 这里还需要后端的接口进行配合，每个角色分配一个value -->
            <el-option label="医生" value="Y"> </el-option>
            <el-option label="普通用户" value="N"></el-option>
            <el-option label="管理员" value="N"></el-option>
            </el-select>
            <!-- 在input中放入小图标 -->
            <el-checkbox v-model="single">记住密码</el-checkbox> 
            <!-- <h4 @click="register">没有账号？点击注册</h4> -->
            <!-- <div class="passwordForget">
              <h4>忘记密码?</h4>
                 </div>
                 <div class="registerLogin">
                 <h4 @click="register">没有账号？点击注册</h4>
                 </div> -->
                 <el-button class="login" round type="primary" @click="login">登陆</el-button>
          </el-form-item>
        </el-form>
        <!-- 使用element-ui定义form表单，在样式上做一些调整 -->
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      labelPosition: "right",
      form: {
        phone: "",
        code: "",
      },
      // 定义data对象form，用来保存phone和code
    };
  },
  methods: {
    postCode() {
      this.$http("/shiro/getCode", "get", {
        phone: this.form.phone,
      }).then((res) => {
        this.$message({
          message: `验证码是` + res.result,
          type: "success",
        });
        // console.log(res);
      });
      // 请求发送验证码接口，传递的参数是用户输入的电话号码，成功后弹出收到的验证码
    },
    login() {
      this.$http("/shiro/login", "post", this.form).then((res) => {
        console.log(res.suc);
        if (res.suc == true) {
          this.$message({
            message: "登陆成功",
            type: "success",
          });
          // 点击登录按钮，请求登录接口，发送的参数是手机号码和用户输入的验证码，在请求成功后弹登陆成功的弹窗
          // this.$store.commit("setLogin", true);
          // 登陆成功后把login通过store里的方法存储在storage和store的state中
          setTimeout(() => {
            this.$router.push({
              name: "doctor",
            });
          }, 1500);
        } else {
          this.$message({
            message: "登陆失败",
            type: "error",
          });
          // this.$store.commit("setLogin", false);
        }
      });
    },
  },
};
</script>
<style lang="scss" scope>
body,
html {
  width: 100%;
  height: 100%;
}
#login {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: left;
  align-items: center;
  min-height: 700px;
  min-width: 1200px;
  .login_img {
    height: 100%;
    width: 60%;
    position: absolute;
    background: url(../assets/img/login2.png) right center;

    left: 0px;
    .login_tit {
      display: flex;
      margin-left: 200px;
      font-size: 30px;
      color: #fff;
      div:nth-child(1) {
        width: 0;
        height: 0;
        border-bottom: 50px solid #16dcb8;
        border-left: 30px solid transparent;
      }
      div:nth-child(2) {
        background: #16dcb8;
        width: 230px;
        text-align: center;
        line-height: 50px;
      }
      div:nth-child(3) {
        width: 0;
        height: 0;
        border-top: 50px solid #16dcb8;
        border-right: 30px solid transparent;
      }
    }
  }
  .login_con {
    width: 94%;
    height: 80%;
    background: #f2f3f7;
    overflow: hidden;

    .login_form {
      width: 300px;
      float: right;
      margin-right: 200px;
      margin-top: 100px;

      h3 {
        width: 120px;
        font-size: 22px;
        color: #30c0e0;
        border-bottom: 2px solid #30c0e0;
        padding-bottom: 20px;
        margin-bottom: 30px;
        font-weight: normal;
      }
      h4 {
        font-size: 15px;
        color: #30c0e0;
        font-weight: normal;
        padding-left: 10px;
        margin-top: -7px;
      }

      .el-input__inner {
        border-radius: 20px;
      }
      .el-button.login {
        width: 100%;
        background: #30c0e0;
        margin-top: 25px;
        border: none;
      }
      .phone {
        width: 65%;
      }
      .getnum {
        width: 90px;
        font-size: 12px;
        background: #30c0e0;
        padding: 12px;
        margin-left: 5px;
      }
    }
  }
}
</style>

