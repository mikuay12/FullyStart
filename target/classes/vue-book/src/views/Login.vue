<script setup>
import axios from "axios";
import { User, Lock } from "@element-plus/icons-vue";
import { ref } from "vue";
import { ElMessage } from "element-plus";
//控制注册与登录表单的显示， 默认显示注册
const isRegister = ref(false);
const loginconditions = ref({
  username: "",
  password: "",
});
const registerconditions = ref({
  username: "",
  password: "",
  repassword: "",
});

const checkRePassword = (rule, value, callback) => {
  if (value === "") {
    callback(new ErrorEvent("请再次确认密码"));
  } else if (value !== registerconditions.value.password) {
    callback(new ErrorEvent("请确保两次密码一致"));
  } else {
    callback();
  }
};
const rules = {
  username: [{ required: true, message: "请输入用户名", trigger: "blur" }],
  password: [{ required: true, message: "请输入密码", trigger: "blur" }],
  repassword: [{ validator: checkRePassword, trigger: "blur" }],
};
import { useRouter } from "vue-router";
const router = useRouter();
/* const login = function () {
  axios
    .get("http://localhost:8080/Login", {
      params: { ...loginconditions.value },
    })
    .then((result) => {
      router.push("/");
      console.log(result.data);
    })
    .catch((err) => {
      console.log(err);
    });
}; */
/* const register = function () {
  axios
    .get("http://localhost:8080/Register", {
      params: { ...registerconditions.value },
    })
    .then((result) => {
      console.log(result.data);
    })
    .catch((err) => {
      console.log(err);
    });
}; */
import { useRegisterService, useLoginService } from "@/api/user";
const register = async () => {
  console.log("这是registerconditions：", registerconditions.value);
  let result = await useRegisterService(registerconditions.value);
  /*   if (result.code == 0) {
    alert(result.message ? result.message : "注册成功");
  } else {
    alert("注册失败");
  } */
  /* alert(result.message ? result.message : "注册成功"); */
  ElMessage.success(result.message ? result.message : "注册成功");
};
const login = async () => {
  console.log("这是loginconditions：", loginconditions.value);
  let result = await useLoginService(loginconditions.value);
  console.log(result.code);
  if (result.code == 0) {
    /* alert(result.message ? result.message : "登录成功"); */
    ElMessage.success(result.message ? result.message : "登录成功");
    router.push("/");
  } else {
    alert("登录失败");
  }
};
</script>

<template>
  <el-row class="login-page">
    <el-col :span="12" class="bg"></el-col>
    <el-col :span="6" :offset="3" class="form">
      <!-- 注册表单 -->
      <el-form
        ref="form"
        size="large"
        autocomplete="off"
        v-if="isRegister"
        :model="registerconditions"
        :rules="rules"
      >
        <el-form-item>
          <h1>注册</h1>
        </el-form-item>
        <el-form-item prop="username">
          <el-input
            :prefix-icon="User"
            placeholder="请输入用户名"
            v-model="registerconditions.username"
          ></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            :prefix-icon="Lock"
            type="password"
            placeholder="请输入密码"
            v-model="registerconditions.password"
          ></el-input>
        </el-form-item>
        <el-form-item prop="repassword">
          <el-input
            :prefix-icon="Lock"
            type="password"
            placeholder="请输入再次密码"
            v-model="registerconditions.repassword"
          ></el-input>
        </el-form-item>
        <!-- 注册按钮 -->
        <el-form-item>
          <el-button
            class="button"
            type="primary"
            auto-insert-space
            @click="register"
          >
            注册
          </el-button>
        </el-form-item>
        <el-form-item class="flex">
          <el-link type="info" :underline="false" @click="isRegister = false">
            ← 返回
          </el-link>
        </el-form-item>
      </el-form>
      <!-- 登录表单 -->
      <el-form ref="form" size="large" autocomplete="off" v-else>
        <el-form-item>
          <h1>登录</h1>
        </el-form-item>
        <el-form-item>
          <el-input
            :prefix-icon="User"
            placeholder="请输入用户名"
            v-model="loginconditions.username"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
            name="password"
            :prefix-icon="Lock"
            type="password"
            placeholder="请输入密码"
            v-model="loginconditions.password"
          ></el-input>
        </el-form-item>
        <el-form-item class="flex">
          <div class="flex">
            <el-checkbox>记住我</el-checkbox>
            <el-link type="primary" :underline="false">忘记密码？</el-link>
          </div>
        </el-form-item>
        <!-- 登录按钮 -->
        <el-form-item>
          <el-button
            class="button"
            type="primary"
            v-on:click="login"
            auto-insert-space
          >
            登录
          </el-button>
        </el-form-item>
        <el-form-item class="flex">
          <el-link type="info" :underline="false" @click="isRegister = true">
            注册 →
          </el-link>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>

<style lang="scss" scoped>
/* 样式 */
.login-page {
  height: 100vh;
  background-color: #fff;

  .bg {
    background: url("@/assets/book5.jpg") no-repeat 100% center / auto,
      url("@/assets/login_bg.jpg") no-repeat center / cover;
    border-radius: 0 20px 20px 0;
  }

  .form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    user-select: none;

    .title {
      margin: 0 auto;
    }

    .button {
      width: 100%;
    }

    .flex {
      width: 100%;
      display: flex;
      justify-content: space-between;
    }
  }
}
</style>
