<template>
  <div>
    <div class="bg">
      <div class="login_box">
        <!-- 头像区域 -->
        <div class="avatar_box">
          <img src="~assets/images/wyy.png" alt />
        </div>
        <div class="right-top">
          <i class="el-icon-minus"></i>
          <i class="el-icon-close"></i>
        </div>
        <!-- 登录表单区 -->
        <el-form
          ref="loginFormRef"
          label-width="0px"
          :rules="loginFormRules"
          class="login_form"
          :model="loginForm"
        >
          <!-- 用户名 -->
          <el-form-item prop="username">
            <el-input
              placeholder="请输入账号"
              prefix-icon="el-icon-user"
              v-model="loginForm.phone"
            ></el-input>
          </el-form-item>
          <!-- 密码 -->
          <el-form-item prop="password">
            <el-input
              placeholder="请输入密码"
              prefix-icon="el-icon-lock"
              v-model="loginForm.password"
              type="password"
            ></el-input>
          </el-form-item>
          <!-- 按钮 -->
          <el-form-item class="btns">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetLoginForm">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
    <div class="bgBox"></div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      //登录表单的数据绑定对象
      loginForm: {
        phone: "15731214911",
        password: "yuxi1314",
      },
      //表单验证规则对象
      loginFormRules: {
        phone: [
          { required: true, message: "请输入手机号", trigger: "blur" }, //blur失去焦点
        ],

        password: [
          { required: true, message: "请输入登录密码", trigger: "blur" },
        ],
      },
      //是否显示登录对话框
      // loginShow: true,
    };
  },
  created() {},
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    async login() {
      const res = await this.$http.post("login/cellphone", this.loginForm);
      if (res.statusText !== "OK") {
        this.loginShow = false;
        return this.$message.error(
          "登陆失败,您输入的手机号或密码有误，请重新输入！"
        );
      }
      if (res.statusText === "OK") {
        this.$message.success("登陆成功！");
        //token只应在当前网站打开期间生效,所以将token保存在sessionStorage中
        // window.sessionStorage.setItem("token", res.token);
        // this.$store.commit("getProfile", res.data.profile);
        window.localStorage.setItem("getP", JSON.stringify(res.data.profile));
        this.$store.commit("getLogin", true);
        return this.$router.push("/home/find/geXing");
      } else {
        return console.log(res);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.bgBox {
  width: 100%; /*宽度铺满屏幕*/
  padding-top: 52.734%; /*图片高度除以宽度，得到此值*/
  // background: url("~assets/images/download/壁纸3.jpg") no-repeat; /*两个center分别为水平和垂直方向的对齐方式*/
  // background-color: rgb(77, 27, 9);
  background-size: 100%; /*背景水平铺满*/
  filter: blur(8px); /*虚化值，越大越模糊*/
}
.bg {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  // background: url("~assets/images/download/壁纸3.jpg") no-repeat;
  background-size: 100% 100%;
  // background: rgba(255, 255, 255, 0.3);
  z-index: 9999;
  // filter: blur(13px);
}
.bigBox {
  margin: 0px auto;
  height: 400px;
  width: 550px;
  background-color: rgb(247, 107, 107);
  border-radius: 10px;
}

.login_box {
  margin-top: -60px;
  width: 450px;
  height: 300px;
  // background: rgb(89, 38, 38);
  border-radius: 10px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  // background-color: #eee;
  // border: 3px solid #ccc;
  box-shadow: 0 0 10px #ddd;
}

.avatar_box > img {
  position: absolute;
  left: 50%;
  top: 0%;
  transform: translate(-50%, -50%);
  height: 65px;
  // border-radius: 46px;
  // border: 1px solid #eee;
  // box-shadow: 0 0 4px #ddd;
}
.right-top {
  position: absolute;
  top: 0;
  right: 0;
  width: 60px;
  height: 36px;
  // background-color: blue;
}
.right-top {
  float: right;
  display: flex;
  justify-content: center;
  align-items: center;
  i {
    display: inline-block;
    width: 25px;
  }
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 10px 50px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>