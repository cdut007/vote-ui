<template>

  <div class="mod-demo-ueditor">

    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <el-form-item label="选手名称" prop="username">
        <el-input v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="手机号" prop="mobile">
        <el-input v-model="form.mobile"></el-input>
      </el-form-item>

      <el-form-item label="选手描述" prop="description">
        <el-input v-model="form.description"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="commit('form')">提交</el-button>
      </el-form-item>
    </el-form>



  </div>
</template>

<script>

  export default {
    data () {
      return {
        dialogVisible: false,
        form: {
          subject: '',
          begin: '',
          end: ''
        },
        rules: {
          username: [
            { required: true, message: '请输入选手姓名', trigger: 'blur' },
            { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
          ]
        }
      }
    },
    mounted () {

    },
    methods: {
      getContent () {

      },
      commit (formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl('/participant/save'),
              method: 'post',
              data: this.$http.adornData({
                'username': this.form.username,
                'mobile': this.form.mobile,
                'description': this.form.description,
                'activityId': this.$route.params.activity.id
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message('添加选手成功!');
              } else {
                this.$message.error(data.msg)
              }
            })
          }else{
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss">
  .mod-demo-ueditor {
    position: relative;
    z-index: 510;
    > .el-alert {
      margin-bottom: 10px;
    }
  }
</style>
