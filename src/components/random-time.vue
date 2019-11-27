<template>
  <div>
    <el-form>
      <el-row>
        <el-col :span="12">
          <el-form-item label="选择时间范围：" required>
            <el-date-picker
              v-model="dateRange"
              value-format="timestamp"
              type="datetimerange"
              :picker-options="pickerOptions"
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
              align="right">
            </el-date-picker>
          </el-form-item>
        </el-col>
        <el-col :span="12">
          <el-form-item label="选择生成时间格式：">
            <el-radio-group v-model="dateFormat">
              <el-radio label="timestamp">时间戳(1574345754000)</el-radio>
              <el-radio label="YYYY-MM-DD hh:mm:ss">标准格式(2019-10-30 16:26:31)</el-radio>
            </el-radio-group>
          </el-form-item>
        </el-col>
      </el-row>
      <el-form-item>
        <a href="http://momentjs.cn/docs/#/displaying/format/" target="_blank">支持的自定义时间格式请点击这里查看！</a>
      </el-form-item>
      <el-form-item label="自定义时间格式">
        <el-input v-model="dateFormat"></el-input>
      </el-form-item>
      <el-form-item label="请输入需要生成的数据条数" required>
        <el-input v-model="dataLength"></el-input>
      </el-form-item>
      <el-form-item label="随机时间戳">
        <el-input v-model="randomTime" type="textarea" class="data-result"></el-input>
      </el-form-item>
    </el-form>
    <el-button @click="generateTime" type="primary">生成时间戳</el-button>
  </div>
</template>

<script>
const moment = require('moment')
export default {
  name: 'random-time',
  data () {
    return {
      pickerOptions: {
        shortcuts: [{
            text: '最近一周',
            onClick(picker) {
              const end = new Date()
              const start = new Date()
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
              picker.$emit('pick', [start, end])
            }
          }, {
            text: '最近一个月',
            onClick(picker) {
              const end = new Date()
              const start = new Date()
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
              picker.$emit('pick', [start, end])
            }
          }, {
            text: '最近三个月',
            onClick(picker) {
              const end = new Date()
              const start = new Date()
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90)
              picker.$emit('pick', [start, end])
          }
        }]
      },
      dateRange: [new Date().getTime() - 3600 * 1000 * 24 * 30, new Date()],              // 日期范围, 用于随机生成日期
      dateFormat: 'timestamp',    // 日期格式
      mockData: '',
      dataLength: 1,
      randomTime: ''              // 随机时间戳
    }
  },
  methods: {
    /**
     * 生成特定格式的时间
     * @param timestamp 时间戳
     * @param type 时间格式类型
     */
    generateFormatTime (timestamp, type) {
      if (type === 'timestamp') return timestamp
      return moment(timestamp).format(type)
    },
    /**
     * 生成指定数量时间戳
     */
    generateTime () {
      let timeList = []
      for (let i = 0; i < this.dataLength; i++) {
        let timestamp = Math.floor(Math.random() * (this.dateRange[1] - this.dateRange[0]) + this.dateRange[0])
        timeList.push(this.generateFormatTime(timestamp, this.dateFormat))
      }
      this.randomTime = timeList.join('\n')
    }
  }
}
</script>

<style lang="stylus">
.data-result .el-textarea__inner {
  min-height: 300px;
  height: 300px;
}
</style>
