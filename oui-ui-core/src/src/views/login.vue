<template>
  <div>
    <div style="text-align: center" class="logo">
      <img src="/icons/logo.png" alt="" style="width: 130px;">
    </div>
    <el-card :header="$t('Authorization Required')" class="oui-login">
      <el-alert :title="$t('Wrong username or password given!')" type="error" effect="dark" :closable="false" v-if="!valid"></el-alert>
      <el-form ref="login" :model="form" label-width="100px" label-position="left" :rules="rules">
        <el-form-item :label="$t('Username')" prop="username">
          <el-input v-model="form.username" prefix-icon="el-icon-user-solid" :placeholder="$t('Please input username')"
                    @keyup.enter.native="handleLogin"></el-input>
        </el-form-item>
        <el-form-item :label="$t('Password')" prop="password">
          <el-input v-model="form.password" show-password prefix-icon="el-icon-lock" :placeholder="$t('Please input password')"
                    @keyup.enter.native="handleLogin"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="handleLogin" style="width: 70%">{{ $t('Login') }}</el-button>
          <el-button type="warning" @click="reset">{{ $t('Reset') }}</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
  
</template>

<script>
import ElCard from "../../node_modules/element-ui/packages/card/src/main.vue";

export default {
    components: {ElCard},
    data() {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          {required: true, message: this.$t('This field is required')}
        ]
      },
      valid: true
    }
  },
  methods: {
    handleLogin() {
      this.$refs['login'].validate(valid => {
        if (valid) {
          this.$session.login(this.form.username, this.form.password).then(valid => {
            if (valid) {
              this.$session.updateACLs().then(() => {
                this.$router.push('/');
              });
              return;
            }

            this.valid = false;
          });
        }
      });
    },
    reset() {
      this.$refs['login'].resetFields();
      this.valid = true;
    }
  },
  created() {
    this.$session.logout();
    this.$getLang();
  }
}
</script>

<style lang="scss">
.oui-login {
  width: 500px;
  top: 50%;
  left: 50%;
  position: absolute;
  transform: translate(-50%, -50%);

  .el-alert {
    margin-bottom: 10px;
  }

}
.logo{
  position: absolute;
  top: 23%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
