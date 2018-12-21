<template>
  <div>
    <Table :loading="loading" :columns="columns1" :data="data1"></Table>
    <!--翻页功能-->
    <Page :total="sumProduct" style="float:right;margin-top:10px;" :current="currentPage" :page-size="pageCount" @on-change="pagePro" show-sizer @on-page-size-change="changePageCount"/>
    <!--/翻页功能-->
    <!--详情-->
    <Modal
        title="酒店详情"
        v-model="otherParam.detailOne"
        :styles="{top: '20px',width:'950px',height:'550px',left:'120px'}"
        :mask-closable="false"
         class-name="vertical-center-modal-detail">
        <div class="modal_left" style="height:600px;margin-top:0px;">
            <img :src="datasCurrent.hotel_img" class="model_img"/>
            <h3 style="color:#5389FF;"><Icon type="md-home" />酒店攻略</h3>
            {{datasCurrent.hotel_help}}
        </div>
        <div class="modal_right" style="height:600px;margin-top:0px;">
            <h2>{{datasCurrent.hotel_name}}
                <!--修改按钮-->
            <Button type="success" @click="otherParam.gameIntroduceModal = true,otherParam.detailOne = false" style="margin-left:20px;">修改</Button>
            </h2>
            <p><Icon type="md-alarm" />&nbsp;酒店编号：{{datasCurrent.hotel_id}}</p>
            <p><Icon type="md-attach" />&nbsp;酒店设施：{{datasCurrent.hotel_facility}}</p>
            <p><Icon type="md-alarm" />&nbsp;入住时间：{{datasCurrent.hotel_intime}}</p>
            <p><Icon type="md-alarm" />&nbsp;退房时间：{{datasCurrent.hotel_outtime}}</p>
            <p><Icon type="ios-star" />&nbsp;星级：{{datasCurrent.hotel_rate}}</p>
            <p><Icon type="md-pin" />&nbsp;酒店位置：{{datasCurrent.hotel_location}}</p>
            <p><Icon type="md-alarm" />&nbsp;建造年份：{{datasCurrent.hotel_buildtime}}</p>
            <p><Icon type="md-alarm" />&nbsp;装修年份：{{datasCurrent.hotel_rebuildtime}}</p>
            <p><Icon type="ios-water-outline" />&nbsp;酒店规模：{{datasCurrent.hotel_rooms}}</p>
            <p><Icon type="ios-thumbs-up" />&nbsp;酒店评分：{{datasCurrent.hotel_score}}</p>
            <p><Icon type="md-attach" />&nbsp;酒店服务：{{datasCurrent.hotel_service}}</p>
            <p><Icon type="md-attach" />&nbsp;酒店设施：{{datasCurrent.facility}}</p>
            <p><Icon type="md-attach" />&nbsp;主要设施：{{datasCurrent.main_facility}}</p>
            <p><Icon type="md-attach" />&nbsp;房间设施：{{datasCurrent.room_facility}}</p>
        </div>
    </Modal>
    <!--/详情-->
    <---test2018-12-21->

    <!--修改-->
<Modal title="修改" v-model="otherParam.gameIntroduceModal"
:scrollable=false
:footer-hide=true
:fullscreen=true
class-name="vertical-center-modal">
       <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
        <FormItem label="酒店id" prop="formValidate.hotel_id" style="display:none">
            <Input  placeholder="酒店id" v-model="formValidate.hotel_id"/>
        </FormItem>
        <FormItem label="酒店名称" prop="formValidate.hotel_name">
            <Input  placeholder="酒店名称" v-model="formValidate.hotel_name"/>
        </FormItem>
        <FormItem label="酒店位置" prop="formValidate.hotel_location">
            <Input  placeholder="酒店位置" v-model="formValidate.hotel_location"/>
        </FormItem>

        <FormItem label="酒店星级" prop="formValidate.hotel_rate">
            <Input  placeholder="酒店星级" v-model="formValidate.hotel_rate" />
        </FormItem>
         <FormItem label="入住时间" prop="formValidate.hotel_intime">
             <Input  placeholder="入住时间" v-model="formValidate.hotel_intime" style="width:200px;"/>
        </FormItem>
         <FormItem label="时间退房" prop="formValidate.hotel_outtime">
              <Input  placeholder="退房时间" v-model="formValidate.hotel_outtime" style="width:200px;"/>
        </FormItem>

        <FormItem label="建造时间" prop="formValidate.hotel_buildtime">
             <Input  placeholder="建造时间" v-model="formValidate.hotel_buildtime" style="width:200px;"/>
        </FormItem>
         <FormItem label="装修时间" prop="formValidate.hotel_rebuildtime">
              <Input  placeholder="装修时间" v-model="formValidate.hotel_rebuildtime" style="width:200px;"/>
        </FormItem>

        <!-- <FormItem>
            <Row>
                <Col span="11">
                    <FormItem label="建造时间" prop="formValidate.hotel_buildtime">
                        <DatePicker type="date" placeholder="Select date" v-model="formValidate.hotel_buildtime"></DatePicker>
                    </FormItem>
                </Col>
                <Col span="2" style="text-align: center">-</Col>
                <Col span="11">
                    <FormItem label="装修时间" prop="formValidate.hotel_rebuildtime">
                        <DatePicker type="date" placeholder="Select date" v-model="formValidate.hotel_rebuildtime"></DatePicker>
                    </FormItem>
                </Col>
            </Row>
        </FormItem> -->

         <FormItem label="酒店规模" prop="formValidate.hotel_rooms">
            <Input  placeholder="酒店规模" v-model="formValidate.hotel_rooms"/>
        </FormItem>

         <FormItem label="主要设施" prop="formValidate.main_facility">
            <CheckboxGroup v-model="formValidate.main_facility">
                <!--遍历的应该是所有，对应的会默认选中-->
                 <Checkbox v-for="(str,i) in datasCurrent.main_facility" :key="i" :label="str">
                    {{str}}
                </Checkbox>
            </CheckboxGroup>
        </FormItem>

         <FormItem label="酒店服务" prop=".formValidatehotel_service">
                 <CheckboxGroup v-model="formValidate.hotel_service">
                 <Checkbox v-for="(str,i) in datasCurrent.hotel_service" :key="i" :label="str">
                    {{str}}
            </Checkbox>
            </CheckboxGroup>
        </FormItem>

         <FormItem label="酒店设施" prop="formValidate.hotel_facility">
            <CheckboxGroup v-model="formValidate.hotel_facility">
                 <Checkbox v-for="(str,i) in datasCurrent.hotel_facility" :key="i" :label="str">
                    {{str}}
                 </Checkbox>
            </CheckboxGroup>
        </FormItem>

         <FormItem label="房间设施" prop="formValidate.room_facility">
            <CheckboxGroup v-model="formValidate.room_facility">
                <Checkbox v-for="(str,i) in datasCurrent.room_facility" :key="i" :label="str">
                    {{str}}
                </Checkbox>
            </CheckboxGroup>
        </FormItem>
        <FormItem label="酒店攻略" prop="formValidate.hotel_help">
            <Input v-model="formValidate.hotel_help" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="Enter something..." />
        </FormItem>
         <Upload
        multiple
        type="drag"
        action="//jsonplaceholder.typicode.com/posts/">
        <div style="padding: 20px 0">
            <Icon type="ios-cloud-upload" size="52" style="color: #3399ff"></Icon>
            <p>上传图片</p>
        </div>
    </Upload>
        <FormItem>
            <Button type="primary" @click="handleSubmit('formValidate')">提交</Button>
            <Button @click="handleReset('formValidate')" style="margin-left: 8px">Reset</Button>
        </FormItem>
    </Form>
    </Modal>
    <!--/修改-->

     <!-- 删除modal -->
    <Modal v-model="modal2" width="360">
      <p slot="header" style="color:#f60;text-align:center">
        <Icon type="ios-information-circle"></Icon>
        <span>删除</span>
      </p>
      <div style="text-align:center">
        <p>
          是否确定删除,删除后数据不可恢复！
          <span style="color:#367CE2;font-size:16px;"></span>
        </p>
      </div>
      <div slot="footer">
        <Button type="error" size="large" long  @click="deleteHotel()">删除</Button>
      </div>
    </Modal>
    <!--/删除-->
  </div>
</template>
<script>
export default {
  data () {
    return {
      currentPage: 1,
      pageCount: 20, // 默认每页20条
      sumProduct: 0, // 一共有多少条数据
      hotel_id: null,
      loading: true,
      datas: Object,
      datasCurrent: Object,
      delId: null,
      gameIntroduceModal: false,
      modal2: false,
      otherParam: {
        detailOne: false,
        gameIntroduceModal: false
      },
      // 表单
      formValidate: {
        hotel_id: '',
        hotel_name: '',
        hotel_location: '',
        hotel_rate: '',
        // hotel_score:'',
        hotel_intime: '',
        hotel_outtime: '',
        hotel_buildtime: '',
        hotel_rebuildtime: '',
        hotel_rooms: '',
        main_facility: [],
        hotel_service: [],
        hotel_facility: [],
        room_facility: [],
        hotel_help: ''
      },
      ruleValidate: {
        hotel_name: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        hotel_location: [
          { required: true, message: '不能为空', trigger: 'blur' },
          { type: 'email', message: '不能为空', trigger: 'blur' }
        ],
        hotel_rate: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        hotel_intime: [
          { required: true, message: '不能为空', trigger: 'change' }
        ],
        hotel_outtime: [
          { required: true, message: '不能为空', trigger: 'change' }
        ],
        hotel_buildtime: [
          { required: true, type: 'date', message: 'Please select the date', trigger: 'change' }
        ],
        hotel_rebuildtime: [
          { required: true, type: 'date', message: 'Please select the date', trigger: 'change' }
        ],
        hotel_location: [
          { required: true, message: '不能为空', trigger: 'change' }
        ],
        main_facility: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        hotel_service: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        hotel_facility: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        room_facility: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        hotel_help: [
          { required: true, message: 'Please enter a personal introduction', trigger: 'blur' },
          { type: 'string', min: 20, message: 'Introduce no less than 20 words', trigger: 'blur' }
        ]
      },
      // 结束表单
      columns1: [
        // {
        //   type: "selection",
        //   width: 32
        // },
        {
          title: '编号',
          width: 32,
          type: 'selection',
          key: 'hotel_id'
        },
        {
          title: '图片',
          key: 'hotel_img',
          align: 'center',
          width: 150,
          render: (h, params) => {
            return h('img', {
              domProps: {
                src: params.row.hotel_img
              },
              style: { width: '120px', margin: '0px', borderRadius: '5px' }
            })
          }
        },
        {
          title: '酒店位置',
          key: 'hotel_location',
          align: 'center'
        },
        {
          title: '酒店名称',
          key: 'hotel_name',
          align: 'center'
        },
        { title: '酒店攻略',
          key: 'hotel_help',
          tooltip: true
        },
        {
          title: '酒店星级',
          align: 'center',
          key: 'hotel_rate'
        },
        {
          title: '入住时间',
          align: 'center',
          key: 'hotel_intime'
        },
        {
          title: '退房时间',
          align: 'center',
          key: 'hotel_outtime'
        },
        {
          title: '操作',
          hotel_id: this.hotel_id,
          width: 150,
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h(
                'Button',
                {
                  props: {
                    type: 'primary',
                    size: 'small'
                  },
                  style: {
                    marginRight: '5px',
                    marginTop: '3px',
                    height: '30px',
                    width: '90px'
                  },
                  on: {
                    click: () => {
                      // 打开模态框
                      console.log(this.otherParam.detailOne)
                      this.otherParam.detailOne = true
                      // 将datasCurrent设值
                      this.datasCurrent = this.datas.product[params.index]
                      // 日期封装
                      //    this.datasCurrent.hotel_buildtime=this.datasCurrent.hotel_buildtime
                      //    this.datasCurrent.hotel_rebuildtime=this.datasCurrent.hotel_rebuildtime
                      // 数组封装
                      this.formValidate = this.datas.product[params.index]
                         this.datasCurrent.room_facility=this.datasCurrent.room_facility.split(",")
                         this.datasCurrent.main_facility=this.datasCurrent.main_facility.split(",")
                         this.datasCurrent.hotel_service=this.datasCurrent.hotel_service.split(",")
                         this.datasCurrent.hotel_facility=this.datasCurrent.hotel_facility.split(",")
                      // 为表单赋值
                      this.formValidate.room_facility = this.datasCurrent.room_facility
                      this.formValidate.main_facility = this.datasCurrent.main_facility
                      this.formValidate.hotel_service = this.datasCurrent.hotel_service
                      this.formValidate.hotel_facility = this.datasCurrent.hotel_facility
                      console.log(this.formValidate.hotel_facility)
                    }
                  }
                },
                '详情'
              ),
              h(
                'Button',
                {
                  props: {
                    type: 'error',
                    size: 'small'
                  },
                  style: {
                    marginRight: '5px',
                    height: '30px',
                    marginTop: '3px',
                    width: '90px'
                  },
                  on: {
                    click: () => {
                    //   this.deleteHote(params.row.hotel_id)
                      this.delId = params.row.hotel_id
                      this.modal2 = true
                      // this.remove(params.row.gid);
                    }
                  }
                },
                '删除'
              )
            ])
          }
        }
      ],
      data1: [
        {
          hotel_id: []
        }
      ]
    }
  },
  methods: {
    // 获取所有信息分页
    getListGameByPage () {
      this.axios
        .get('http://localhost:8080/adminproject/allHotelByPage', {
          params: {
            currentPage: this.currentPages,
            pageCount: this.pageCount
          }
        })
        .then(response => {
          console.log(response.data)
          //   this.gid = response.data.gid;
          // 返回数据
          this.data1 = response.data.product
          // 取消加载状态
          this.loading = false
          // 给所有的记录条数赋值
          this.sumProduct = response.data.sumProduct
          this.datas = response.data
        })
    },
    handleSubmit (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          // this.$Message.success('Success!');
          this.hotelUpdate()
        } else {
          // this.$Message.error('Fail!');
        }
      })
    },
    handleReset (name) {
      this.$refs[name].resetFields()
    },
    // 修改

    hotelUpdate () {
      let config = {
        headers: {
          'Content-Type': 'multipart/form-data'
          // "Accept":"*/*",
          // "Access-Control-Allow-Methods":" . (isset($_GET['allow_method']) ? $_GET['allow_method'] : 'OPTIONS')"
        }
      }
      this.axios.post('http://localhost:8080/adminproject/updateHotel', {
        params: {
          hotel_id: this.formValidate.hotel_id,
          hotel_name: this.formValidate.hotel_name,
          hotel_location: this.formValidate.hotel_location,
          hotel_rate: this.formValidate.hotel_rate,
          // hotel_score:,
          hotel_intime: this.formValidate.hotel_intime,
          hotel_outtime: this.formValidate.hotel_outtime,
          hotel_buildtime: this.formValidate.hotel_buildtime,
          hotel_rebuildtime: this.formValidate.hotel_rebuildtime,
          hotel_rooms: this.formValidate.hotel_rooms,
          main_facility: this.formValidate.main_facility,
          hotel_service: this.formValidate.hotel_service,
          hotel_facility: this.formValidate.hotel_facility,
          room_facility: this.formValidate.room_facility,
          hotel_help: this.formValidate.hotel_help
        }
      })
        .then(response => {
          console.log(response)
          this.alertSE(response.data)
          // 重新发送axios请求刷新页面
          this.getListGameByPage()
          this.otherParam.gameIntroduceModal = false
        })
        .catch(function (error) {
          console.log(error)
        })
        .then(function () {

          // always executed
        })
    },
    alertSE (result) {
      if (result == 'success') {
        this.$Message.success('修改成功!')
      } else {
        this.$Message.error('服务器正忙')
      }
    },
    deleteHotel () {
      this.axios.delete('http://localhost:8080/adminproject/removeHotel/' + this.delId, {
      })
        .then(response => {
          alert(this.delId)
          if (response.data == 'success') {
            this.cl()
            // 重新发送axios请求刷新页面
            this.getListGameByPage()
          } else {
            this.erro()
          }
          console.log(response.data)
        })
        .catch(function (error) {
          this.erro()
        })
        .then(function () {
          // always executed
        })
    },
    // 翻页功能
    pagePro (currentPages) {
      // 给当前页赋值
      this.currentPages = currentPages
      console.log(this.currentPages)
      // 发送ajax请求调用
      this.getListGameByPage()
    },
    // 改变显示条数
    changePageCount (pageCount) {
      // 给显示的记录条数赋值
      this.pageCount = pageCount
      // 发送ajax请求调用
      this.getListGameByPage()
    },
    cl () {
      // 关闭模态框
      this.modal2 = false
      this.$Modal.success({
        title: 'success',
        content: '成功',
        okText: 'OK',
        cancelText: 'Cancel'
      })
    },
    erro () {
      this.modal2 = false
      this.$Modal.error({
        title: '失败',
        content: '请稍后再试'
      })
    }
  },
  mounted () {
    this.getListGameByPage()
  }
}
</script>
<style>
    .modal_left{
        width:380px;
        height:200px;
        float:left;
        /* 图片中的字体 */
        color:#3C3C3C;
        font-size:14px;
        /* height:150px; */
    }
    .modal_right{
        /* width:230px; */
        width:500px;
        height:170px;
        float:right;
        font-size: 14px;
        color:#2F2F2F;
        /* height:500px; */
    }
    .model_img{
        width:100%;
        border-radius:10px ;
        margin:0 auto;
    }
    .ivu-modal-body{
        /* height:550px; */
    }
    .vertical-center-modal{
        /* height:1800px; */
    }
</style>
