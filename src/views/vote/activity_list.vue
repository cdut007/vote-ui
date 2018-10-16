<template>

  <div class="mod-activity-list">
    <el-row :gutter="12">
      <el-col  :span="8" v-for="item in activity_list" :key="o">
        <el-card :body-style="{ padding: '4px'}" >
          <img src="~@/assets/img/Annualmeeting10.jpg" class="image">
          <div style="padding: 14px;">
            <span>{{item.subject}}</span>
            <div class="bottom clearfix">
              <time class="time">{{ item.begin|formatDate }} 至 {{ item.end|formatDate }}</time>
              <div style="margin-top: 14px;float: right; padding: 0;">
                <el-button type="success" @click="edit(item)" plain>编辑</el-button>
                <el-button type="primary" @click="create(item)">添加选手</el-button>
                <el-button  @click="view(item)" plain>查看选手</el-button>
              </div>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>

  </div>
</template>

<script>

  function formatDate(date,fmt){
    if(/(y+)/.test(fmt)){
      fmt = fmt.replace(RegExp.$1, (date.getFullYear()+'').substr(4-RegExp.$1.length));
    }
    let o = {
      'M+': date.getMonth()+1,
      'd+': date.getDate(),
      'h+': date.getHours(),
      'm+': date.getMinutes(),
      's+': date.getSeconds()
    }
    for(let k in o){
      let str = o[k]+'';
      if(new RegExp(`(${k})`).test(fmt)){
        fmt = fmt.replace(RegExp.$1, (RegExp.$1.length===1)?str:padLeftZero(str));
      }
    }
    return fmt;
  };

  function padLeftZero(str){
    return ('00'+str).substr(str.length);
  }


  export default {
    data () {
      return {
        currentDate: new Date(),
        form: {
          subject: '',
          begin: '',
          end: ''
        },
        activity_list:[]
      }
    },
    filters:{
      formatDate(time){
        let date = new Date(time);
        return formatDate(date,'yyyy-MM-dd hh:mm:ss')
      }
    },
    mounted () {
      this.$http({
        url: this.$http.adornUrl('/activity/list'),
        method: 'get',
        params: this.$http.adornParams({
          'limit': 100,
          'order': 'createTime'
        })
      }).then(({data}) => {
        if (data && data.code === 0) {
          this.activity_list = data.page.list
        } else {
          this.$message.error(data.msg)
        }
      })
    },
    methods: {
      getContent () {

      },
      edit (item) {
        this.$router.push({ name: 'vote-ueditor-edit' , params: {activity: item}})
      },
      create (item) {
        this.$router.push({ name: 'create-participant' , params: {activity: item}})
      },
      view (item) {
        this.$router.push({ name: 'participant-list' , params: {activity: item}})
      }
    }
  }
</script>

<style lang="scss">
  .mod-activity-list {
    position: relative;
    z-index: 510;
    > .el-alert {
      margin-bottom: 10px;
    }
  }
  .el-card{
    margin:10px
  }

  .time {
    font-size: 13px;
    color: #999;
  }

  .bottom {
    margin-top: 13px;
    line-height: 12px;
  }

  .button {
    padding: 0;
    float: right;
  }

  .image {
    width: 100%;
    display: block;
  }

  .clearfix:before,
  .clearfix:after {
    display: table;
    content: "";
  }

  .clearfix:after {
    clear: both
  }
</style>
