<template>
      <div>
          <div id="btn">
              <Button type="primary" @click="modalform=true">增加游记</Button>&nbsp;
             <Button type="primary" @click="removes">删除多个</Button>&nbsp;
             <Input placeholder="请输入内容" style="width: auto" @on-enter="search" v-model="searchVal">
                <Icon type="ios-search" slot="suffix" @on-click="search"/>
            </Input>
          </div>
           <Table :columns="columns" :data="data" @on-select="onSelect"></Table>
        <Page :total="total" size="small" :page-size="pageSize" :page-size-opts="pageSizeOpts" @on-page-size-change="onChangePageSize" @on-change="onChangePage" align="right" style="margin-top:10px" show-elevator show-sizer />

        <!-- 模态框 -->
        <Modal v-model="modalform" title="数据操作">

            <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="120">
                <FormItem label="游记标题" prop="travelNoteTitle">
                    <Input v-model="formValidate.travelNoteTitle" placeholder="游记标题"></Input>
                </FormItem>
                <FormItem label="游记图片地址(url)" prop="travelNoteBanner">
                    <Input v-model="formValidate.travelNoteBanner" placeholder="游记图片地址"></Input>
                </FormItem>
                <FormItem label="游玩开始时间">
                    <Row>
                        <Col span="11">
                            <FormItem prop="travelNoteBeginTime">
                                <DatePicker type="date" placeholder="请选择时间" v-model="formValidate.travelNoteBeginTime"></DatePicker>
                            </FormItem>
                        </Col>
                    </Row>
                </FormItem>
                <FormItem label="游玩时间" prop="travelNotePeriod">
                    <InputNumber :max="12" :min="1" v-model="formValidate.travelNotePeriod"></InputNumber>&nbsp;&nbsp;天
                </FormItem>
                <FormItem label="游玩价格" prop="travelNotePrice">
                    <InputNumber :max="Infinity" :min="0" v-model="formValidate.travelNotePrice"></InputNumber>&nbsp;&nbsp;元
                </FormItem>
                <FormItem label="和谁一起" prop="travelNoteStyle">
                    <RadioGroup  v-model="formValidate.travelNoteStyle" placeholder="本次游记对应的旅游的参与人类型">
                        <Radio label="和朋友">和朋友</Radio>
                        <Radio label="家庭出游">家庭出游</Radio>
                        <Radio label="情侣">情侣</Radio>
                        <Radio label="一个人">一个人</Radio>
                        <Radio label="带孩子">带孩子</Radio>
                        <Radio label="其他">其他</Radio>
                    </RadioGroup >
                </FormItem>
                <FormItem label="目的地" prop="travelNoteDestination">
                    <Input v-model="formValidate.travelNoteDestination" placeholder="游记目的地"></Input>
                </FormItem>
                <FormItem>
                    <Button type="primary" @click="handleSubmit('formValidate')">数据操作</Button>
                    <Button @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>
                </FormItem>
            </Form>
            <p slot="footer" style="color:#f60;text-align:center"></p>
        </Modal>
      </div>
</template>
<script>
import expandRow from './table-expand.vue'
export default {
  inject: ['reload'],
  components: { expandRow },
  data () {
    return {
      address: 'src/mfwVue/components/classification.vue',
      // 表单验证
      formValidate: {
        travelNoteTitle: '',
        travelNoteBanner: '',
        travelNoteBeginTime: '',
        travelNotePeriod: '',
        travelNotePrice: '',
        travelNoteStyle: '',
        // travelNoteBrowses: '',
        // travelNoteDiscuss:'',
        // travelNoteLikes: '',
        // travelNoteCollects:'',
        // travelNoteShares:'',
        travelNoteDestination: ''
      },
      ruleValidate: {
        travelNoteTitle: [
          { required: true, message: '游记标题不能为空', trigger: 'blur' }
        ],
        travelNoteBanner: [
          { required: true, message: '图片地址不能为空', trigger: 'blur' },
          { type: 'url', message: '请输入格式正确的图片地址', trigger: 'blur' }
        ],
        travelNoteBeginTime: [
          { required: true, type: 'date', message: '请选择游玩日期', trigger: 'change' }
        ],
        travelNotePeriod: [
          { required: true, type: 'number', message: '请输入游玩时间', trigger: 'change' }
        ],
        travelNotePrice: [
          { required: true, type: 'number', message: '请输入游玩人均价格', trigger: 'change' }
        ],
        travelNoteStyle: [
          { required: false, type: 'string', message: '本次游记对应的旅游的参与人类型', trigger: 'change' }
        ],
        travelNoteDestination: [
          { required: true, type: 'string', message: '请输入目的地', trigger: 'blur' }
        ]
      },
      // 表格数据
      columns: [
        {
          type: 'selection',
          width: 60,
          align: 'cneter'
        },
        {
          type: 'expand',
          width: 50,
          render: (h, params) => {
            console.log('params:' + params.row)
            return h(expandRow, {
              props: {
                row: params.row
              }
            })
          }
        },
        {
          title: '游记id',
          key: 'travelNoteId'
        },
        {
          title: '游记标题',
          key: 'travelNoteTitle'
        },
        {
          title: '图片',
          key: 'travelNoteBanner',
          render: (h, params) => {
            return h('img', {
              domProps: {
                src: params.row.travelNoteBanner
              },
              style: {
                width: '100px',
                height: '100px',
                margin: '0px',
                borderRadius: '5px' }
            })
          }
        },
        {
          title: '出发时间',
          key: 'travelNoteBeginTime'
        },
        {
          title: '旅行时长',
          key: 'travelNotePeriod'
        },
        {
          title: '人均费用',
          key: 'travelNotePrice'
        },
        {
          title: '操作',
          key: 'action',
          width: 150,
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h('Button', {
                props: {
                  type: 'primary',
                  size: 'small'
                },
                style: {
                  marginRight: '5px'
                },
                on: {
                  click: () => {
                    this.edit(params.row)
                  }
                }
              }, '编辑'),
              h('Button', {
                props: {
                  type: 'error',
                  size: 'small'
                },
                on: {
                  click: () => {
                    this.remove(params.row.travelNoteId)
                  }
                }
              }, '删除')
            ])
          }
        }
      ],
      data: [],
      total: 0, // 商品总数
      pageIndex: 1, // 当前页
      pageSize: 5, // 一页显示条数
      pageSizeOpts: [5, 10, 20], // 每页显示条数切换
      ids: [], // 批量删除数组
      modalform: false,
      searchVal: '',
      ids: []// 批量删除数组
    }
  },
  methods: {
    // modal
    handleSubmit (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          if (this.formValidate.travelNoteId === undefined || this.formValidate.travelNoteId == null) { // 增加
            this.axios({
              url: 'http://localhost:8080/adminproject/travel/insertTravel',
              method: 'post',
              data: {
                travelNoteTitle: this.formValidate.travelNoteTitle,
                travelNoteBanner: this.formValidate.travelNoteBanner,
                travelNoteBeginTime: this.formValidate.travelNoteBeginTime.toString,
                travelNotePeriod: this.formValidate.travelNotePeriod,
                travelNotePrice: this.formValidate.travelNotePrice,
                travelNoteStyle: this.formValidate.travelNoteStyle,
                travelNoteDestination: this.formValidate.travelNoteDestination
              }
            }).then(res => {
              this.reload
              this.modalform = false
              this.getData()
            })
          } else { // 否则就是编辑
            this.axios({
              url: 'http://localhost:8080/adminproject/travel/editTravel',
              method: 'put',
              data: {
                travelNoteId: this.formValidate.travelNoteId,
                travelNoteTitle: this.formValidate.travelNoteTitle,
                travelNoteBanner: this.formValidate.travelNoteBanner,
                travelNoteBeginTime: this.formValidate.travelNoteBeginTime.toLocaleDateString(),
                travelNotePeriod: this.formValidate.travelNotePeriod,
                travelNotePrice: this.formValidate.travelNotePrice,
                travelNoteStyle: this.formValidate.travelNoteStyle,
                travelNoteDestination: this.formValidate.travelNoteDestination
              }
            }).then(res => {
              this.reload
              this.modalform = false
              this.getData()
            })
          }
          this.$Message.title('操作成功!')
        } else {
          this.$Message.error('操作失败!')
        }
      })
    },
    handleReset (name) {
      this.$refs[name].resetFields()
    },
    edit (row) {
      this.formValidate = row
      this.modalform = true
    },
    remove (travelNoteId) {
      // 确认对话框
      this.$Modal.confirm({
        title: '确认操作',
        content: '<p>你确认删除该条记录吗</p>',
        onOk: () => {
          this.axios({
            url: 'http://localhost:8080/adminproject/travel/removeTravel',
            method: 'delete',
            data: { 'travelNoteId': travelNoteId }
          }).then(res => {
            this.reload
            this.getData()
          })
        },
        onCancel: () => {
          this.$Message.info('取消成功')
        }
      })
    },
    // checkbox
    onSelect (selections) {
      var ids = []
      for (var i = 0; i < selections.length; i++) {
        ids.push(selections[i].travelNoteId)
      }
      this.ids = ids.toString()
      console.log(ids)
    },
    removes () {
      console.log(this.ids)
      this.$Modal.confirm({
        title: '确认操作',
        content: '<p>你确定要删除这些记录吗？</p>',
        onOk: () => {
          this.axios({
            url: 'http://localhost:8080/adminproject/travel/removeTravels',
            method: 'post',
            data: {
              ids: this.ids
            }
          }).then(res => {
            this.reload
            console.log(res)
            this.getData()
          })
        },
        onCancel: () => {
          this.$Message.info('取消成功')
        }
      })
    },
    // 搜索
    search () {
      this.$http.post('http://localhost:8080/adminproject/travel/listTravels', {
        searchVal: this.searchVal,
        pageIndex: this.pageIndex,
        pageSize: this.pageSize
      }, {
        emulateJSON: true
      }).then(res => {
        console.log(res)
        console.log(res.data)
        this.total = res.data.total
        this.data = res.data.date
      })
    },
    // 分页
    onChangePage (pageIndex) {
      this.$http.post('http://localhost:8080/adminproject/travel/listTravels', {
        searchVal: this.searchVal,
        pageIndex: pageIndex,
        pageSize: this.pageSize
      }, {
        emulateJSON: true
      }).then(res => {
        this.total = res.data.total
        this.data = res.data.date
      })
    },
    onChangePageSize (i) {
      console.log(i)
      this.pageSize = i
      this.onChangePage(this.pageIndex)
    },
    getData () {
      this.$http.post('http://localhost:8080/adminproject/travel/listTravels', {
        searchVal: this.searchVal,
        pageIndex: this.pageIndex,
        pageSize: this.pageSize
      }, {
        emulateJSON: true
      }).then(res => {
        console.log(res)
        console.log(res.data)
        this.total = res.data.total
        this.data = res.data.date
      })
    }
  },
  mounted () {
    this.getData()
  }
}
</script>
<style scoped>
#btn{
    margin-bottom: 10px;
}
</style>
