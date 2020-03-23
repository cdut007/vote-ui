<template>
  <div class="site-wrapper site-page--login">
    <div class="site-content__wrapper">
      <div class="site-content">
        <div class="brand-info">
          <h2 class="brand-info__text">爱特联投票制作系统</h2>
          <p class="brand-info__intro">提供一套更优的投票解决方案。</p>
        </div>
       <div class="login-main">
         <el-tabs v-model="activeName">
           <el-tab-pane label="用户登录" name="first">
            <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" status-icon>
              <el-form-item prop="userName">
                <el-input v-model="dataForm.userName" placeholder="帐号/手机号"></el-input>
              </el-form-item>
              <el-form-item prop="password">
                <el-input v-model="dataForm.password" type="password" placeholder="密码"></el-input>
              </el-form-item>

              <el-form-item prop="captcha">
                <el-row :gutter="20">
                  <el-col :span="14">
                    <el-input v-model="dataForm.captcha" placeholder="验证码">
                    </el-input>
                  </el-col>
                  <el-col :span="10" class="login-captcha">
                    <img :src="captchaPath" @click="getCaptcha()" alt="">
                  </el-col>
                </el-row>
              </el-form-item>
              <el-form-item>
                <el-button class="login-btn-submit" type="primary" @click="dataFormSubmit()">登录</el-button>
              </el-form-item>
            </el-form>

           </el-tab-pane>
           <el-tab-pane label="注册" name="second">
             <el-form :model="dataForm2" :rules="dataRule" ref="dataForm2" @keyup.enter.native="dataForm2Submit()" status-icon>
               <el-form-item prop="mobile">
                 <el-input v-model="dataForm2.mobile" placeholder="手机号"></el-input>
               </el-form-item>
               <el-form-item prop="password">
                 <el-input v-model="dataForm2.password" type="password" placeholder="密码"></el-input>
               </el-form-item>

               <el-form-item prop="code">
                 <el-input  v-model="dataForm2.code"  clearable placeholder="请输入短信验证码" :maxlength="10" />
                 <el-button  @click="sendVerify" v-if="!sended" class="send-verify">获取验证码</el-button>
                 <el-button  disabled v-if="sended" class="count-verify">{{countButton}}</el-button>
               </el-form-item>

               <el-form-item prop="captcha">
                 <el-row :gutter="20">
                   <el-col :span="14">
                     <el-input v-model="dataForm2.captcha" placeholder="验证码">
                     </el-input>
                   </el-col>
                   <el-col :span="10" class="login-captcha">
                     <img :src="captcha2Path" @click="getCaptcha2()" alt="">
                   </el-col>
                 </el-row>
               </el-form-item>
               <el-form-item>
                 <el-button class="login-btn-submit" type="primary" @click="dataForm2Submit()">注册</el-button>
               </el-form-item>
             </el-form>

           </el-tab-pane>
         </el-tabs>
         <footer style="margin-top: 30%">
           <el-row :gutter="20">
             <el-col :span="12" :offset="6"><a href="https://m.kuaidi100.com" target="_blank" style="margin-left: 20%;text-decoration: underline">快递查询</a></el-col>
           </el-row>

         </footer>
          </div>

      </div>
    </div>

  </div>
</template>

<script>
  import { getUUID } from '@/utils'
  export default {
    data () {
      return {
        dataForm: {
          userName: '',
          password: '',
          uuid: '',
          captcha: ''
        },
        dataForm2: {
          userName: '',
          password: '',
          mobile: '',
          code: '',
          uuid: '',
          captcha: '',
          hash: ''
        },
        sended: false,
        count: 60,
        countButton: '60 s',
        activeName: 'first',
        dataRule: {
          userName: [
            { required: true, message: '帐号不能为空', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '密码不能为空', trigger: 'blur' }
          ],
          mobile: [
            { required: true, message: '手机号不能为空', trigger: 'blur' }
          ],
          captcha: [
            { required: true, message: '验证码不能为空', trigger: 'blur' }
          ]
        },
        captchaPath: '',
        captcha2Path: ''
      }
    },
    created () {
      this.getCaptcha()
      this.getCaptcha2()
    },
    methods: {
      // 提交表单
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/login'),
              method: 'post',
              data: this.$http.adornData({
                'username': this.dataForm.userName,
                'password': this.dataForm.password,
                'uuid': this.dataForm.uuid,
                'captcha': this.dataForm.captcha
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$cookie.set('token', data.token)
                this.$router.replace({ name: 'home' })
              } else {
                this.getCaptcha()
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      sendVerify () {
        this.$refs['dataForm2'].validate((valid) => {
          if (valid) {
            this.sended = true
            this.countDown()
            this.$http({
              url: this.$http.adornUrl('/smsCode'),
              method: 'post',
              data: this.$http.adornData({
                'mobile': this.dataForm2.mobile,
                'uuid': this.dataForm2.uuid,
                'captcha': this.dataForm2.captcha
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm2.hash = data.uuid
              } else {
                this.getCaptcha2()
                this.dataForm2.hash = data.uuid
                this.sended = false
                this.count = 0
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      countDown () {
        let that = this
        if (this.count === 0) {
          this.sended = false
          this.count = 60
          return
        } else {
          this.countButton = this.count + ' s'
          this.count--
        }
        setTimeout(function () {
          that.countDown()
        }, 1000)
      },
      dataForm2Submit () {
        this.$refs['dataForm2'].validate((valid) => {
          if (valid) {
            if (!this.dataForm2.code) {
              this.$message.error('请输入短信验证码')
              return
            }
            this.$http({
              url: this.$http.adornUrl('/register'),
              method: 'post',
              data: this.$http.adornData({
                'mobile': this.dataForm2.mobile,
                'password': this.dataForm2.password,
                'uuid': this.dataForm2.hash,
                'smsCode': this.dataForm2.code,
                'captcha': this.dataForm2.captcha
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$cookie.set('token', data.token)
                this.$router.replace({ name: 'home' })
              } else {
               // this.getCaptcha2()
                this.$message.error(data.msg)
              }
            })
          }
        })
      },
      // 获取验证码
      getCaptcha () {
        this.dataForm.uuid = getUUID()
        this.captchaPath = this.$http.adornUrl(`/captcha.jpg?uuid=${this.dataForm.uuid}`)
      },
      getCaptcha2 () {
        this.dataForm2.uuid = getUUID()
        this.captcha2Path = this.$http.adornUrl(`/captcha.jpg?uuid=${this.dataForm2.uuid}`)
      }
    }
  }
</script>

<style lang="scss">
  .site-wrapper.site-page--login {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(38, 50, 56, .6);
    overflow: hidden;
    &:before {
      position: fixed;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      content: "";
      background-image: url(~@/assets/img/login_bg.jpg);
      background-size: cover;
    }
    .site-content__wrapper {
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      padding: 0;
      margin: 0;
      overflow-x: hidden;
      overflow-y: auto;
      background-color: transparent;
    }
    .site-content {
      min-height: 100%;
      padding: 30px 500px 30px 30px;
    }
    .brand-info {
      margin: 220px 100px 0 90px;
      color: #fff;
    }
    .brand-info__text {
      margin:  0 0 22px 0;
      font-size: 48px;
      font-weight: 400;
      text-transform : uppercase;
    }
    .brand-info__intro {
      margin: 10px 0;
      font-size: 16px;
      line-height: 1.58;
      opacity: .6;
    }
    .login-main {
      position: absolute;
      top: 0;
      right: 0;
      padding: 150px 60px 180px;
      width: 470px;
      min-height: 100%;
      background-color: #fff;
    }
    .login-title {
      font-size: 16px;
    }
    .login-captcha {
      overflow: hidden;
      > img {
        width: 100%;
        cursor: pointer;
      }
    }
    .login-btn-submit {
      width: 100%;
      margin-top: 38px;
    }
  }
</style>
