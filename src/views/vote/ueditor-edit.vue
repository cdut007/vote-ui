<template>

  <div class="mod-demo-ueditor">

    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <el-form-item label="活动名称" prop="subject">
        <el-input v-model="form.subject"></el-input>
      </el-form-item>
      <el-form-item label="活动时间" required>
        <el-col :span="11">
          <el-date-picker type="datetime" placeholder="选择开始时间" v-model="form.begin" style="width: 100%;"></el-date-picker>
        </el-col>
        <el-col class="line" :span="2" style="text-align:center"> - </el-col>
        <el-col :span="11">
          <el-date-picker type="datetime" placeholder="选择结束时间" v-model="form.end" style="width: 100%;"></el-date-picker>
        </el-col>
      </el-form-item>
      <el-form-item label="活动与奖品介绍">
        <script :id="ueId" class="ueditor-box" type="text/plain" style="width: 100%; height: 260px;">
        </script>
        </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="commit('form')">保存</el-button>
      </el-form-item>
    </el-form>




    <!-- 查看 -->
    <!-- <p><el-button @click="getContent()">查看</el-button></p> -->
    <el-dialog
      title="内容"
      :visible.sync="dialogVisible"
      :append-to-body="true">
      {{ ueContent }}
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
  import ueditor from 'ueditor'
  export default {
    data () {
      return {
        ue: null,
        ueId: `J_ueditorBox_${new Date().getTime()}`,
        ueContent: this.$route.params.activity.content,
        dialogVisible: false,
        form: {
          subject: this.$route.params.activity.subject,
          begin: this.$route.params.activity.begin,
          end: this.$route.params.activity.end
        },
        rules: {
          subject: [
            { required: true, message: '请输入活动名称', trigger: 'blur' },
            { min: 3, max: 20, message: '长度在 3 到 20 个字符', trigger: 'blur' }
          ],
          begin: [
            { type: 'date', required: true, message: '请选择开始时间', trigger: 'change' }
          ],
          end: [
            { type: 'date', required: true, message: '请选择结束时间', trigger: 'change' }
          ]
        }
      }
    },
    mounted () {
      this.ue = ueditor.getEditor(this.ueId, {
        // serverUrl: '', // 服务器统一请求接口路径
        zIndex: 3000
      })

      this.ue.ready(() => {
        this.ue.setContent(this.ueContent)
      })
    },
    methods: {
      getContent () {
        this.dialogVisible = true
        this.ue.ready(() => {
          this.ueContent = this.ue.getContent()
        })
      },
      commit (formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {

            this.ue.ready(() => {
              this.ueContent = this.ue.getContent()
              if (!this.ueContent) {
                const h = this.$createElement
                this.$msgbox({
                  title: '消息',
                  message: h('p', null, [
                    h('span', null, '请输入模板'),
                    h('i', { style: 'color: teal' }, '内容')
                  ]),
                  showCancelButton: false,
                  confirmButtonText: '确定',
                  beforeClose: (action, instance, done) => {
                    if (action === 'confirm') {
                      done()
                    } else {
                      done()
                    }
                  }
                }).then(action => {

                })
              } else {
                this.$http({
                  url: this.$http.adornUrl('/activity/update'),
                  method: 'post',
                  data: this.$http.adornData({
                    'id': this.$route.params.activity.id,
                    'content': this.ueContent,
                    'subject': this.form.subject,
                    'begin': this.form.begin,
                    'end': this.form.end
                  })
                }).then(({data}) => {
                  if (data && data.code === 0) {
                    this.$message('保存活动成功!');

                  } else {
                    this.$message.error(data.msg)
                  }
                })
              }
            })
          } else {
            console.log('error submit!!')
            return false
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
