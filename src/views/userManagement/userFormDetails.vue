<template>
  <div class="app-container calendar-list-container">
    <div class="userInfo-container">
      <span>用户ID： {{userInfoFormId}}</span>
    </div>
    <el-tabs v-model="activeName" @click="handleTabClick" type="border-card">
      <el-tab-pane name="first" :label="$t('userFormDetails.basicInfoContent')">
        <!--基本信息填写内容-->
        <div class="filter-container">
          <el-button v-if="ad_level.indexOf('1')>-1" class="filter-item" type="primary" icon="document" @click="handleDownload(1)">导出</el-button>
        </div>
        <div class="el-table-container">
          <el-table :key='tableKey1' :data="basicList" v-loading="listLoading" :element-loading-text="$t('utils.loadText')" border fit highlight-current-row style="width: 100%">
            <el-table-column align="center" type="index" width="55"></el-table-column>
            <el-table-column align="center" :label="$t('userManagement.questionNumber')" width="65">
              <template scope="scope">
                <span>{{scope.row.id}}</span>
              </template>
            </el-table-column>
            <el-table-column align="left" :label="$t('userManagement.question')">
              <template scope="scope">
                <span >{{scope.row.question}}</span>
              </template>
            </el-table-column>
            <el-table-column align="left" :label="$t('userManagement.answer')">
              <template scope="scope">
                <template v-if="scope.row.id==1||scope.row.id==2||scope.row.id==3||scope.row.id==4||scope.row.id==5||scope.row.id==7||scope.row.id==12">
                  <span>{{scope.row.answer}}</span>
                </template>
                <template v-if="scope.row.id==6">
                  <span>{{scope.row.answer.type}}:</span>
                  <span v-html="scope.row.answer.scope"></span>
                </template>
                <template v-if="scope.row.id==8">
                  <span>同住人数：{{scope.row.answer.num}}；</span>
                  <span>同住类型：{{scope.row.answer.type}}</span>
                </template>
                <template v-if="scope.row.id==9">
                  <p v-for="item in scope.row.answer">
                    <span>年龄：{{item.age}}；</span>
                    <span>性别：{{item.sex}}；</span>
                    <span>关系：{{item.relation}}；</span>
                  </p>
                </template>
                <template v-if="scope.row.id==10">
                  <p v-for="item in scope.row.answer">
                    <span>范围：{{item.scope}}；</span>
                    <span>频率：{{item.hz}}</span>
                  </p>
                </template>
                <template v-if="scope.row.id==11">
                  <p v-for="item in scope.row.answer">
                    <span>范围：{{item.scope}}；</span>
                    <span>频率：{{item.hz}}</span>
                  </p>
                </template>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </el-tab-pane>
      <el-tab-pane name="second" :label="$t('userFormDetails.logInfoContent')">
        <!--接触日志填写内容-->
        <div class="filter-container">
          <el-button v-if="ad_level.indexOf('1')>-1" class="filter-item" type="primary" icon="document" @click="handleDownload(2)">导出</el-button>
        </div>
        <div class="el-table-container">
          <el-table :key='tableKey2' :data="logList" v-loading="listLoading" :element-loading-text="$t('utils.loadText')" border fit highlight-current-row style="width: 100%">
            <el-table-column type="index" width="55"></el-table-column>
            <el-table-column align="center" :label="$t('userManagement.questionNumber')" width="65">
              <template scope="scope">
                <span>{{scope.row.id}}</span>
              </template>
            </el-table-column>
            <el-table-column align="left" :label="$t('userManagement.question')">
              <template scope="scope">
                <span>{{scope.row.question}}</span>
              </template>
            </el-table-column>
            <el-table-column align="left" :label="$t('userManagement.answer')" >
              <template scope="scope">
                <template v-if="scope.row.id==1||scope.row.id==2||scope.row.id==6||scope.row.id==7||scope.row.id==10||scope.row.id==12||scope.row.id==14||scope.row.id==15||scope.row.id==16">
                  <span>{{scope.row.answer}}</span>
                </template>
                <template v-if="scope.row.id==3">
                    <span v-for="item in scope.row.answer">{{item.age}}；</span>
                </template>
                <template v-if="scope.row.id==4">
                  <p v-for="item in scope.row.answer">
                    <span>年龄：{{item.age}}；</span>
                    <span>性别：{{item.sex}}；</span>
                    <span>接触时间：{{item.cTime}}；</span>
                    <span :index="item.relationIndex">关系：{{item.relation}}；</span>
                    <span>接触类型：{{item.contactType}}；</span>
                    <span v-html="'总时间：'+item.contacttime?item.contacttime: ''+'；'"></span>
                    <span>接触地点：{{item.contactLocation}}；</span>
                    <span>平时的接触频率：{{item.contactHz}}；</span>
                    <span>提交时间：{{item.submitTime | parseTime('{y}-{m}-{d} {h}:{i}:{s}')}}；</span>
                  </p>
                </template>
                <template v-if="scope.row.id==5">
                  <span v-if="scope.row.answer.state=='是' || scope.row.answer.state=='Yes'">是</span>
                  <span v-else>{{scope.row.answer.state}}；估计遗漏数量：{{scope.row.answer.num}}；</span>
                </template>
                <template v-if="scope.row.id==8">
                  <p v-for="item in scope.row.answer">
                    <span v-if="item.type == '其他'">其他：{{item.name}}；</span>
                    <span v-else>种类：{{item.type}}；</span>
                    <span>数量：{{item.num}}；</span>
                  </p>
                </template>
                <template v-if="scope.row.id==9">
                  <p v-for="item in scope.row.answer">
                    <span v-if="item.type == '其他'">其他：{{item.name}}；</span>
                    <span v-else>种类：{{item.type}}；</span>
                    <span>频率：{{item.hz}}；</span>
                  </p>
                </template>
                <template v-if="scope.row.id==11">
                  <p v-for="item in scope.row.answer">
                    <span>动物种类：{{item.type}}；</span>
                    <span>数量：{{item.num}}；</span>
                    <span>接触时间：{{item.contacttime}}；</span>
                    <span>接触地点：{{item.contactLocation}}；</span>
                    <span>接触频率：{{item.contactHz}}；</span>
                  </p>
                </template>
                <template v-if="scope.row.id==13">
                  <span>分数：{{scope.row.answer.grade}}；</span>
                  <span>理由：{{scope.row.answer.reson}}</span>
                </template>
              </template>
            </el-table-column>
            <el-table-column align="center" :label="$t('utils.submitTime')" >
              <template scope="scope">
                <span>{{scope.row.submitTime | parseTime('{y}-{m}-{d} {h}:{i}:{s}')}}</span>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </el-tab-pane>
      <el-tab-pane name="third" label="定位数据记录">
        <!--定位数据记录-->
        <div class="filter-container">
          <el-button v-if="ad_level.indexOf('1')>-1" class="filter-item" type="primary" icon="document" @click="handleDownload(3)">导出</el-button>
        </div>
        <div class="el-table-container">
          <el-table :key='tableKey3' height="450" :data="localLogList" v-loading="listLoading" :element-loading-text="$t('utils.loadText')" border fit highlight-current-row style="width: 100%">
            <el-table-column align="center" type="index" width="85" label="INDEX"></el-table-column>
            <el-table-column align="center" label="DATE">
              <template scope="scope">
                <span v-if="scope.row.nowtime">{{scope.row.nowtime | filterfomentDateYMD}}</span>
                <span v-else>无</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="TIME">
              <template scope="scope">
                <span v-if="scope.row.nowtime">{{scope.row.nowtime | filterfomentDateHIS}}</span>
                <span v-else>无</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="LATITUDE">
              <template scope="scope">
                <span>{{scope.row.locationlog.lat.toFixed(6)}}</span>
              </template>
            </el-table-column>
            <el-table-column align="center" label="LONGITUDE">
              <template scope="scope">
                <span>{{scope.row.locationlog.lon.toFixed(6)}}</span>
              </template>
            </el-table-column>
          </el-table>
          <div class="pagination-container">
            <div class="block">
              <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="1"
                :page-sizes="[10, 20, 50, 100, 500, total]"
                :page-size="listQuery.length"
                layout="total, sizes, prev, pager, next"
                :total="total">
              </el-pagination>
            </div>
          </div>
        </div>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
  import waves from '@/directive/waves/index' // 水波纹指令
  import { parseTime } from '@/utils'
  import { mapGetters } from 'vuex'

  export default {
    name: 'userFormDetails',
    directives: {
      waves
    },
    data() {
      return {
        basicList: [],
        logList: [],
        localLogList: [],
        userInfoFormId: '',
        userInfoForm: {},
        total: null,
        listLoading: true,
        activeName: 'first',
        listQuery: {
          phoneid: undefined,
          id: undefined,
          start: 0,
          length: 10,
        },
        tableKey1: 1,
        tableKey2: 2,
        tableKey3: 3,
      }
    },
    filters: {
      filterfomentDateYMD(value) {
        if (value) {
          const date = new Date(value)
          const formatObj = {
            y: date.getFullYear(),
            m: date.getMonth() + 1,
            d: date.getDate().toString().length > 1 ? date.getDate() : `0${date.getDate()}`,
            h: date.getHours().toString().length > 1 ? date.getHours() : `0${date.getHours()}`,
            i: date.getMinutes().toString().length > 1 ? date.getMinutes() : `0${date.getMinutes()}`,
            s: date.getSeconds().toString().length > 1 ? date.getSeconds() : `0${date.getSeconds()}`,
          }
          return `${formatObj.y}${formatObj.m}${formatObj.d}`
        } else {
          return `无`
        }
      },
      filterfomentDateHIS(value) {
        if (value) {
          const date = new Date(value)
          const formatObj = {
            y: date.getFullYear(),
            m: date.getMonth() + 1,
            d: date.getDate().toString().length > 1 ? date.getDate() : `0${date.getDate()}`,
            h: date.getHours().toString().length > 1 ? date.getHours() : `0${date.getHours()}`,
            i: date.getMinutes().toString().length > 1 ? date.getMinutes() : `0${date.getMinutes()}`,
            s: date.getSeconds().toString().length > 1 ? date.getSeconds() : `0${date.getSeconds()}`,
          }
          return `${formatObj.h}${formatObj.i}${formatObj.s}`
        } else {
          return `无`
        }
      },
    },
    created() {
      if (sessionStorage.getItem('userInfoForm')) {
        const userInfoForm = JSON.parse(sessionStorage.getItem('userInfoForm'))
        this.userInfoForm = userInfoForm
        this.userInfoFormId = userInfoForm.id
        this.listQuery.phoneid = userInfoForm.phoneinfo
        this.listQuery.id = userInfoForm.id
        this.getAnswer()
        this.getLocation()
      } else {
        this.$router.push({
          path: '/userManagement/index'
        })
      }
    },
    computed: {
      ...mapGetters([
        'roles',
        'ad_level'
      ])
    },
    methods: {
      handleTabClick(tab, event) {
        console.log(tab, event)
      },
      getAnswer() {
        this.$http.get('/getAnswer', { params: { id: this.userInfoFormId }}).then(response => {
          this.listLoading = false
          const data = response.data
          if (data.status) {
            const locanswer = data.data[0].locanswer
            const answerlog = data.data[0].answerlog
            if (locanswer) {
              const locanswerArr = JSON.parse(locanswer)
              for (const i in locanswerArr) {
                const node = locanswerArr[i]
                node.question = this.$t('basicInfoQuestions.q' + (node.id))
              }
              this.basicList = locanswerArr.sort((a, b) => {
                return a.id - b.id
              })
            } else {
              this.basicList = []
            }
            if (answerlog) {
              const answerlogArr = JSON.parse(answerlog)
              for (const i in answerlogArr) {
                const node = answerlogArr[i]
                node.question = this.$t('logQuestions.q' + (node.id))
                if (node.id === 8 || node.id === 9) {
                  node.answer.sort((a, b) => {
                    return a.index - b.index
                  })
                }
              }
              this.logList = answerlogArr.sort((a, b) => {
                return a.id - b.id
              })
            } else {
              this.logList = []
            }
          }
        }, response => {
          this.listLoading = false
          this.$message.error(`错误信息：${response}`)
        })
      },
      getLocation() {
        this.listLoading = true
        this.$http.get('/getLocalLog', { params: this.listQuery }).then(response => {
          this.listLoading = false
          const data = response.data
          if (data.data.length > 0) {
            this.localLogList = data.data.map((value, index, array) => {
              value.locationlog = JSON.parse(value.locationlog)
              return value
            })
          }
          this.total = data.data1[0].num
        }, response => {
          this.listLoading = false
          this.$message.error(`错误信息：${response}`)
        })
      },
      handleSizeChange(val) {
        this.listQuery.start = 0
        this.listQuery.length = val
        this.getLocation()
      },
      handleCurrentChange(val) {
        this.listQuery.start = 0
        this.listQuery.start = this.listQuery.length * (val - 1)
        this.getLocation()
      },
      handleDownload(sig) {
        require.ensure([], () => {
          const { export_json_to_excel } = require('vendor/Export2Excel')
          switch (sig) {
            case 1: {
              const tHeader = ['题号', '题目', '答案']
              const filterVal = ['id', 'question', 'answer']
              const basicListArr = this.basicList.concat()
              const resultArr = []
              for (const i in basicListArr) {
                const node = JSON.parse(JSON.stringify(basicListArr[i]))
                const id = node.id
                let answer = node.answer
                if (typeof (answer) === 'object') {
                  answer = JSON.parse(JSON.stringify(node.answer))
                }
                let answerStr = ''
                if (answer) {
                  switch (id) {
                    case 6: {
                      answerStr = `${answer.type}:${answer.scope}`
                      break
                    }
                    case 8: {
                      answerStr = `同住人数：${answer.num ? answer.num : answer.num === 0 ? 0 : ''}；\r\n同住类型：${answer.type || ''}`
                      break
                    }
                    case 9: {
                      for (const i in answer) {
                        const node = answer[i]
                        answerStr += `年龄：${node.age ? node.age : node.age === 0 ? 0 : ''}；性别：${node.sex || ''}; 关系：${node.relation || ''}\r\n`
                      }
                      break
                    }
                    case 10: {
                      for (const i in answer) {
                        const node = answer[i]
                        answerStr += `范围：${node.scope || ''}；频率：${node.hz || ''}\r\n`
                      }
                      break
                    }
                    case 11: {
                      for (const i in answer) {
                        const node = answer[i]
                        answerStr += `范围：${node.scope || ''}；频率：${node.hz || ''}\r\n`
                      }
                      break
                    }
                    default:
                      answerStr = answer
                  }
                } else {
                  answerStr = `未填写`
                }
                resultArr.push({
                  id: node.id,
                  question: node.question,
                  answer: answerStr,
                })
              }
              const data = this.formatJson(filterVal, resultArr)
              export_json_to_excel(tHeader, data, '基本信息填写问卷信息')
              break
            }
            case 2: {
              const tHeader2 = ['题号', '题目', '答案', '提交时间']
              const filterVal2 = ['id', 'question', 'answer', 'submitTime']
              const logListArr = this.logList.concat()
              const resultArr = []
              for (const i in logListArr) {
                const node = JSON.parse(JSON.stringify(logListArr[i]))
                const id = node.id
                const answer = node.answer
                let answerStr = ''
                if (answer) {
                  switch (id) {
                    case 3: {
                      for (const i in answer) {
                        const node = answer[i]
                        answerStr += `${node.age}；\r\n`
                      }
                      break
                    }
                    case 4:
                      for (const i in answer) {
                        const node = answer[i]
                        answerStr += `年龄：${node.age ? node.age : node.age === 0 ? 0 : ''}；性别：${node.sex || ''}；接触时间：${node.cTime || ''}；关系：${node.relation || ''}；接触类型：${node.contactType || ''}；总时间：${node.contacttime || ''}；接触地点：${node.contactLocation || ''}；平时的接触频率:${node.contactHz || ''}；提交时间:${parseTime(node.submitTime || '')}；\r\n`
                      }
                      break
                    case 5:
                      if (answer.index === 0) {
                        answerStr = answer.state || ''
                      } else {
                        answerStr = `没有；估计遗漏数量：${answer.num || ''}`
                      }
                      break
                    case 8:
                      answerStr = `种类：${answer.type || ''}；数量：${answer.num || ''}`
                      break
                    case 9:
                      for (const i in answer) {
                        const node = answer[i]
                        answerStr += `种类：${node.type || ''}；频率：${node.hz || ''}\r\n`
                      }
                      break
                    case 11: {
                      for (const i in answer) {
                        const node = answer[i]
                        answerStr += `动物种类：${node.type || ''}；数量：${node.num || ''}；接触时间：${node.contacttime || ''}；接触地点：${node.contactLocation || ''}；\r\n`
                      }
                      break
                    }
                    case 13: {
                      answerStr = `分数：${answer.grade || 0}；\r\n 理由：${answer.reson || ''}`
                      break
                    }
                    default:
                      answerStr = answer
                  }
                } else {
                  answerStr = `未填写`
                }
                resultArr.push({
                  id: node.id,
                  question: node.question,
                  answer: answerStr,
                  submitTime: node.submitTime,
                })
              }
              const data2 = this.formatJson(filterVal2, resultArr)
              export_json_to_excel(tHeader2, data2, '接触日志填写问卷信息')
              break
            }
            case 3: {
              const tHeader3 = ['INDEX', 'DATE', 'TIME', 'LATITUDE', 'LONGITUDE']
              const filterVal3 = ['index', 'date1', 'date2', 'lat', 'lon']
              const localLogListArr = this.localLogList.concat()
              const resultArr = []
              for (const i in localLogListArr) {
                const node = JSON.parse(JSON.stringify(localLogListArr[i]))
                resultArr.push({
                  index: Number(i) + 1,
                  date1: node.nowtime ? this.fomentDateYMD(node.nowtime) : `无`,
                  date2: node.nowtime ? this.fomentDateHIS(node.nowtime) : `无`,
                  lat: node.locationlog.lat.toFixed(6),
                  lon: node.locationlog.lon.toFixed(6),
                })
              }
              const data3 = this.formatJson(filterVal3, resultArr)
              export_json_to_excel(tHeader3, data3, '定位数据记录信息')
              break
            }
            default: {
              this.$message.error(`错误信息：生成表格失败，错误的参数`)
            }
          }
        })
      },
      formatJson(filterVal, jsonData) {
        return jsonData.map(v => filterVal.map(j => {
          if (j === 'submitTime') {
            return parseTime(v[j])
          } else {
            return v[j]
          }
        }))
      },
      fomentDateYMD(value) {
        if (value) {
          const date = new Date(value)
          const formatObj = {
            y: date.getFullYear(),
            m: date.getMonth() + 1,
            d: date.getDate().toString().length > 1 ? date.getDate() : `0${date.getDate()}`,
            h: date.getHours().toString().length > 1 ? date.getHours() : `0${date.getHours()}`,
            i: date.getMinutes().toString().length > 1 ? date.getMinutes() : `0${date.getMinutes()}`,
            s: date.getSeconds().toString().length > 1 ? date.getSeconds() : `0${date.getSeconds()}`,
          }
          return `${formatObj.y}${formatObj.m}${formatObj.d}`
        } else {
          return `无`
        }
      },
      fomentDateHIS(value) {
        if (value) {
          const date = new Date(value)
          const formatObj = {
            y: date.getFullYear(),
            m: date.getMonth() + 1,
            d: date.getDate().toString().length > 1 ? date.getDate() : `0${date.getDate()}`,
            h: date.getHours().toString().length > 1 ? date.getHours() : `0${date.getHours()}`,
            i: date.getMinutes().toString().length > 1 ? date.getMinutes() : `0${date.getMinutes()}`,
            s: date.getSeconds().toString().length > 1 ? date.getSeconds() : `0${date.getSeconds()}`,
          }
          return `${formatObj.h}${formatObj.i}${formatObj.s}`
        } else {
          return `无`
        }
      },
    }
  }
</script>
<style lang="less" scoped>
  .userInfo-container{padding-bottom: 10px;}
 .el-table-container{
   .el-table .cell span{text-align: left}
   table{
   }
 }
</style>
