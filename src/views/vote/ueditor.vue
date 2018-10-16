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
          <p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><strong><span style="color: rgb(229, 102, 0); font-size: 18px;">投票模板标题</span></strong></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><strong><span style="color: rgb(229, 102, 0); font-size: 18px;"><br/></span></strong></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><strong><span style="color: rgb(229, 102, 0); font-size: 18px;">活动介绍</span></strong></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="line-height: 1.5;"><span style="font-size: 14px;">1.	点击首页导航栏&nbsp;<span style="color: rgb(229, 51, 51); font-size: 18px;">“我要报名”&nbsp;</span>即可参与活动，报名时请提交真实姓名和电话号码作为后期领奖凭证，参与活动。</span><br/></span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="line-height: 1.5; font-size: 14px;">2.	照片需符合<span style="font-size: 18px;">&nbsp;</span><span style="font-size: 18px; color: rgb(229, 51, 51);">“XX活动”&nbsp;</span>主题，可自己进行创新，每张照片需附上“参赛宣言”上传活动页面，审核后即可参与活动</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><strong><span style="color: rgb(229, 102, 0); font-size: 18px;">投票规则</span></strong></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="line-height: 1.5; font-size: 14px;">1. 参赛者上传信息，严禁盗用他人照片等信息</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="font-size: 14px;">2.	禁止参赛作品出现任何非法内容、言论不当等行为</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="font-size: 14px;">3.	严禁任何作弊行为，系统发现会自动对票数清空</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="font-size: 14px;">4. 作品设计著作权，肖像权等法律责任由活动参与者自行承担法律责任</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="line-height: 1.5;"><span style="font-size: 14px;">5.	以上行为一经发现，主办方有权删除照片并</span>&nbsp;</span><strong><span style="color: rgb(229, 51, 51);">取消参赛资格</span></strong></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="font-size: 14px;">6.	禁止加入诱导关注/诱导分享内容</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><br/></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><strong><span style="font-size: 18px; color: rgb(229, 102, 0);">奖品说明</span></strong></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="line-height: 1.5; font-size: 14px;">1. 第一名：奖励 XXXX 一个</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="font-size: 14px;">2. 第二名：奖励 XXXX 一个</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="font-size: 14px;">3. 第三名：奖励 XXXX 一个</span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><strong><span style="color: rgb(229, 102, 0); font-size: 18px;">主办单位</span></strong></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="color: rgb(229, 102, 0); font-size: 18px;"><span style="font-size: 14px; color: rgb(51, 51, 51);">1. 主办方：XXX公司</span><span style="font-size: 14px; color: rgb(51, 51, 51);"></span></span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="color: rgb(229, 102, 0); font-size: 18px;"><span style="font-size: 14px; color: rgb(51, 51, 51);">2. 地址：成都市高新区XXX街XXX路X号<br/></span></span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="color: rgb(229, 102, 0); font-size: 18px;"><span style="font-size: 14px; color: rgb(51, 51, 51);">3. 电话：028-66666666<br/></span></span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><br/></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><br/></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="color: rgb(229, 102, 0); font-size: 18px;"><span style="font-size: 14px; color: rgb(51, 51, 51);"><strong><br/></strong></span></span></p><p style="overflow-wrap: break-word; margin-top: 1px; margin-bottom: 1px; font-family: &quot;sans serif&quot;, tahoma, verdana, helvetica; white-space: normal;"><span style="color: rgb(229, 102, 0); font-size: 18px;"><span style="font-size: 14px; color: rgb(51, 51, 51);"><strong><span style="color: rgb(229, 51, 51);">本活动最终解释权归XX公司所有！</span></strong></span></span></p><p><br/></p>

        </script>
        </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="commit('form')">立即创建</el-button>
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
        ueContent: '',
        dialogVisible: false,
        form: {
          subject: '',
          begin: '',
          end: ''
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
                  url: this.$http.adornUrl('/activity/save'),
                  method: 'post',
                  data: this.$http.adornData({
                    'content': this.ueContent,
                    'subject': this.form.subject,
                    'begin': this.form.begin,
                    'end': this.form.end
                  })
                }).then(({data}) => {
                  if (data && data.code === 0) {
                    this.$message('创建活动成功!');
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
