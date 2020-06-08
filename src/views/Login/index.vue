<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="ulist">
        <li
          v-for="item in Arr"
          :key="item.id"
          :class="{'current':item.current}"
          @click="toggle(item)"
        >{{item.txt}}</li>
      </ul>
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        label-width="100px"
        class="login-Form"
      >
        <el-form-item prop="email" class="item-form">
          <label>邮箱</label>
          <el-input  v-model="ruleForm.email" autocomplete="off" size="medium"></el-input>
        </el-form-item>
        <el-form-item prop="checkPass" class="item-form">
          <label>密码</label>
          <el-input type="password" maxlength="20" minlength="6" v-model="ruleForm.checkPass" autocomplete="off" size="medium"></el-input>
        </el-form-item>
        <el-form-item prop="code" class="item-form">
          <label>验证码</label>
          <el-row :gutter="20">
            <el-col :span="12">
              <el-input v-model.number="ruleForm.code" maxlength="6" minlength="6" size="medium"></el-input>
            </el-col>
            <el-col :span="12">
              <el-button type="success" class="block">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button
            type="danger"
            @click="submitForm('ruleForm')"
            class="login-btn block"
            style="width:100%;"
          >提交</el-button>
          <!-- <el-button @click="resetForm('ruleForm')">重置</el-button> -->
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    var checkcode = (rule, value, callback) => {
      let res=/^[A-Za-z0-9]{6}$/
      if (value === "") {
        return callback(new Error("验证码不能为空"));
      }else if(!res.test(value)){
        return callback(new Error("验证码格式有误"))
      }
      else
      {
        return callback();
      }
    };
    var validatePass = (rule, value, callback) => {
      let res = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
      if (value === "") {
        callback(new Error("请输入邮箱"));
      } else if (!res.test(value)) {
        callback(new Error("邮箱有误"));
      } else {
        if (this.ruleForm.checkPass !== "") {
          this.$refs.ruleForm.validateField("checkPass");
        }
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      let res=/^(?=.*\d)(?=.*[a-z])[a-zA-Z\d]{6,20}$/
      if (value === "") {
        callback(new Error("请输入密码"));
      } else if (!res.test(value)) {
        callback(new Error("密码为6-20的数字+字母"));
      } else {
        callback();
      }
    };
    return {
      Arr: [
        {
          txt: "登录",
          current: true
        },
        {
          txt: "注册",
          current: false
        }
      ],
      ruleForm: {
        email: "",
        checkPass: "",
        code: ""
      },
      rules: {
        email: [{ validator: validatePass, trigger: "blur" }],
        checkPass: [{ validator: validatePass2, trigger: "blur" }],
        code: [{ validator: checkcode, trigger: "blur" }]
      }
    };
  },
  created() {},
  methods: {
    toggle(data) {
      this.Arr.forEach(element => {
        element.current = false;
        console.log(element.current);
      });
      data.current = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
#login {
  height: 100vh;
  background-color: #344a5f;

  .login-wrap {
    width: 330px;
    margin: auto;
  }

  .ulist {
    list-style: none;
    text-align: center;
    li {
      display: inline-block;
      width: 88px;
      line-height: 36px;
      font-size: 14px;
      color: #fff;
      border-radius: 2px;
      cursor: pointer;
    }
    .current {
      background-color: rgba(0, 0, 0, 0.1);
    }
  }
  .login-Form {
    margin-top: 29px;
    label {
      display: block;
      font-size: 14px;
      color: white;
    }
    .item-form {
      margin-bottom: 13px;
    }
    .block {
      display: block;
      width: 100%;
    }
    .login-btn {
      margin-top: 19px;
    }
  }
}
</style>