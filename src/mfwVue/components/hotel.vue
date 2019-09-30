<template>
  <div>
    酒店位置：<Input v-model="searchs.hotel_location" placeholder="请输入酒店位置" @on-enter="searchOrAll()" class="search"/>
    &nbsp;&nbsp;&nbsp;
    酒店名称：<Input v-model="searchs.hotel_name" placeholder="请输入酒店名称" @on-enter="searchOrAll()" class="search"/>
    <Button type="primary" @click="searchOrAll()" style="margin-left:10px;" shape="circle" icon="ios-search"></Button>
    <br/><br/>
    <Table :loading="loading" :columns="columns1" :data="data1"></Table>
    <!--翻页功能-->
    <Page :total="sumProduct" style="float:right;margin-top:10px;" :current="currentPage" :page-size="pageCount"
          @on-change="pagePro" show-sizer @on-page-size-change="changePageCount"/>
    <!--/翻页功能-->
    <!--详情-->
    <Modal
      :title="datasCurrent.hotelName"
      v-model="otherParam.detailOne"
      :styles="{top: '20px',width:'950px',height:'550px',left:'120px'}"
      :mask-closable="false"
      class-name="vertical-center-modal-detail">
      <div class="modal_left" style="height:600px;margin-top:0px;">
        <img :src="datasCurrent.hotelImg" class="model_img"/>
        <h3 style="color:#5389FF; font-size:20px;margin-top:10px">
          <Icon type="md-home"/>
          酒店攻略
        </h3>
        <label class="help_value">{{datasCurrent.hotelHelp}}</label>
      </div>
      <div class="modal_right" style="height:600px;margin-top:0px;">
        <h2>
          <!--修改按钮-->
          <Button type="success" @click="otherParam.gameIntroduceModal = true,otherParam.detailOne = false"
                  style="float:right;border-radius: 100px;">修改
          </Button>
        </h2>
        <p>
          <Icon type="logo-octocat" />&nbsp;<label class="title_name">酒店编号：</label>{{datasCurrent.hotelId}}
        </p>
        <p>
          <Icon type="ios-star"/>&nbsp;<label class="title_name">星级：</label>{{datasCurrent.hotelRate}}星级酒店
        </p>
        <p>
          <Icon type="ios-thumbs-up"/>&nbsp;<label class="title_name">酒店评分：</label>{{datasCurrent.hotelScore}}分
        </p>
        <p>
          <Icon type="md-pin"/>&nbsp;<label class="title_name">酒店位置：</label>{{datasCurrent.hotelLocation}}
        </p>
        <p>
          <Icon type="ios-tennisball" />&nbsp;<label class="title_name">酒店规模：</label>{{datasCurrent.hotelRooms}}
        </p>
        <p>
          <Icon type="ios-alarm" /><label class="title_name">入住时间：</label>{{datasCurrent.hotelIntime}}
        </p>
        <p>
          <Icon type="ios-alarm" />&nbsp;<label class="title_name">退房时间：</label>{{datasCurrent.hotelOuttime}}
        </p>
        <p>
          <Icon type="md-build" />&nbsp;<label class="title_name">建造年份：</label>{{datasCurrent.hotelBuildtime}}
        </p>
        <p>
          <Icon type="md-flame" /><label class="title_name">装修年份：</label>{{datasCurrent.hotelRebuildtime}}
        </p>
        <p>
          <Icon type="md-attach"/>&nbsp;<label class="title_name">酒店设施：</label>{{datasCurrent.hotelFacility}}
        </p>
        <p>
          <Icon type="md-attach"/>&nbsp;<label class="title_name">酒店服务：</label>{{datasCurrent.hotelService}}
        </p>
        <p>
          <Icon type="md-attach"/>&nbsp;<label class="title_name">酒店设施：</label>{{datasCurrent.hotelFacility}}
        </p>
        <p>
          <Icon type="md-attach"/>&nbsp;<label class="title_name">主要设施：</label>{{datasCurrent.mainFacility}}
        </p>
        <p>
          <Icon type="md-attach"/>&nbsp;<label class="title_name">房间设施：</label>{{datasCurrent.roomFacility}}
        </p>
      </div>
    </Modal>
    <!--/详情-->

    <!--修改-->
    <Modal title="修改" v-model="otherParam.gameIntroduceModal"
           :scrollable=false
           :footer-hide=true
           :fullscreen=true
           class-name="vertical-center-modal">
      <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
        <FormItem label="酒店id" prop="formValidate.hotel_id" style="display:none">
          <Input placeholder="酒店id" v-model="formValidate.hotelId"/>
        </FormItem>
        <FormItem label="酒店名称" prop="formValidate.hotelName">
          <Input placeholder="酒店名称" v-model="formValidate.hotelName"/>
        </FormItem>
        <FormItem label="酒店位置" prop="formValidate.hoteLocation">
          <Input placeholder="酒店位置" v-model="formValidate.hotelLocation"/>
        </FormItem>

        <FormItem label="酒店星级" prop="formValidate.hotelRate">
          <Input placeholder="酒店星级" v-model="formValidate.hotelRate"/>
        </FormItem>
        <FormItem label="入住时间">
          <Row>
            <Col span="11">
              <TimePicker type="time" placeholder="请选择时间" v-model="formValidate.hotelIntime"></TimePicker>
            </Col>
          </Row>
        </FormItem>

        <FormItem label="退房时间">
          <Row>
            <Col span="11">
              <TimePicker type="time" placeholder="请选择时间" v-model="formValidate.hotelOuttime"></TimePicker>
            </Col>
          </Row>
        </FormItem>

        <FormItem label="建造时间" prop="formValidate.hotel_buildtime">
          <DatePicker type="date" placeholder="Select date" v-model="formValidate.hotelBuildtime"></DatePicker>
        </FormItem>
        <FormItem label="装修时间" prop="formValidate.hotel_rebuildtime">
          <DatePicker type="date" placeholder="Select date" v-model="formValidate.hotelRebuildtime"></DatePicker>
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

        <FormItem label="酒店规模" prop="formValidate.hotelRooms">
          <Input placeholder="酒店规模" v-model="formValidate.hotelRooms"/>
        </FormItem>

        <FormItem label="主要设施" prop="formValidate.mainFacility">
          <CheckboxGroup v-model="formValidate.mainFacility">
            <!--遍历的应该是所有，对应的会默认选中-->
            <Checkbox v-for="(str,i) in datasCurrent.mainFacility" :key="i" :label="str">
              {{str}}
            </Checkbox>
          </CheckboxGroup>
        </FormItem>

        <FormItem label="酒店服务" prop=".formValidatehotelService">
          <CheckboxGroup v-model="formValidate.hotelService">
            <Checkbox v-for="(str,i) in datasCurrent.hotelService" :key="i" :label="str">
              {{str}}
            </Checkbox>
          </CheckboxGroup>
        </FormItem>

        <FormItem label="酒店设施" prop="formValidate.hotelFacility">
          <CheckboxGroup v-model="formValidate.hotel_facility">
            <Checkbox v-for="(str,i) in datasCurrent.hotelFacility" :key="i" :label="str">
              {{str}}
            </Checkbox>
          </CheckboxGroup>
        </FormItem>
        <FormItem label="房间设施" prop="formValidate.roomFacility">
          <CheckboxGroup v-model="formValidate.roomFacility">
            <Checkbox v-for="(str,i) in datasCurrent.roomFacility" :key="i" :label="str">
              {{str}}
            </Checkbox>
          </CheckboxGroup>
        </FormItem>
        <FormItem label="酒店攻略" prop="formValidate.hotel_help">
          <Input v-model="formValidate.hotelHelp" type="textarea" :autosize="{minRows: 2,maxRows: 5}"
                 placeholder="Enter something..."/>
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
        <Button type="error" size="large" long @click="deleteHotel()">删除</Button>
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
      hotelId: null,
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
      searchs: {
        hotelLocation: '',
        hotelName: ''
      },
      // 表单
      formValidate: {
        hotelId: '',
        hotelName: '',
        hotelLocation: '',
        hotel_rate: '',
        // hotel_score:'',
        hotelIntime: '',
        hotelOuttime: '',
        hotelBuildtime: '',
        hotelRebuildtime: '',
        hotelRooms: '',
        mainFacility: [],
        hotelService: [],
        hotelFacility: [],
        roomFacility: [],
        hotelHelp: ''
      },
      ruleValidate: {
        hotelName: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        hotelLocation: [
          { required: true, message: '不能为空', trigger: 'blur' },
          { type: 'email', message: '不能为空', trigger: 'blur' }
        ],
        hotelRate: [
          { required: true, message: '不能为空', trigger: 'blur' }
        ],
        hotelIntime: [
          { required: true, message: '不能为空', trigger: 'change' }
        ],
        hotelOuttime: [
          { required: true, message: '不能为空', trigger: 'change' }
        ],
        hotelBuildtime: [
          { required: true, type: 'date', message: 'Please select the date', trigger: 'change' }
        ],
        hotelRebuildtime: [
          { required: true, type: 'date', message: 'Please select the date', trigger: 'change' }
        ],
        hotelLocation: [
          { required: true, message: '不能为空', trigger: 'change' }
        ],
        mainFacility: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        hotelService: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        hotelFacility: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        roomFacility: [
          { required: true, type: 'array', min: 1, message: '最少选择一个', trigger: 'change' },
          { type: 'array', max: 2, message: '不能为空', trigger: 'change' }
        ],
        hotelHelp: [
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
          key: 'hotelId'
        },
        {
          title: '图片',
          key: 'hotelImg',
          align: 'center',
          width: 150,
          render: (h, params) => {
            return h('img', {
              domProps: {
                src: params.row.hotelImg
              },
              style: { width: '55px', margin: '0px', borderRadius: '5px', marginTop: '4px' }
            })
          }
        },
        {
          title: '酒店名称',
          key: 'hotelName',
          align: 'center'
        },
        {
          title: '酒店星级',
          align: 'center',
          key: 'hotelRate',
          render: (h, params) => {
            return h('span', params.row.hotelRate + '星级酒店')
          }
        },
        {
          title: '酒店位置',
          key: 'hotelLocation',
          align: 'center'
        },
        {
          title: '入住时间',
          align: 'center',
          key: 'hotelIntime'
        },
        {
          title: '退房时间',
          align: 'center',
          key: 'hotelOuttime'
        },
        {
          title: '酒店攻略',
          key: 'hotelHelp',
          tooltip: true
        },
        {
          title: '操作',
          hotel_id: this.hotel_id,
          width: 170,
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
                    width: '60px',
                    float: 'left'
                  },
                  on: {
                    click: () => {
                      // 打开模态框
                      console.log(this.otherParam.detailOne)
                      this.otherParam.detailOne = true
                      // 将datasCurrent设值
                      this.datasCurrent = this.datas[params.index]
                      // 日期封装
                      //    this.datasCurrent.hotel_buildtime=this.datasCurrent.hotel_buildtime
                      //    this.datasCurrent.hotel_rebuildtime=this.datasCurrent.hotel_rebuildtime
                      // 数组封装
                      this.formValidate = this.datas[params.index]
                      this.datasCurrent.roomFacility = this.datasCurrent.roomFacility.split(',')
                      this.datasCurrent.mainFacility = this.datasCurrent.mainFacility.split(',')
                      this.datasCurrent.hotelService = this.datasCurrent.hotelService.split(',')
                      this.datasCurrent.hotelFacility = this.datasCurrent.hotelFacility.split(',')
                      // 为表单赋值
                      this.formValidate.roomFacility = this.datasCurrent.roomFacility
                      this.formValidate.mainFacility = this.datasCurrent.mainFacility
                      this.formValidate.hotelService = this.datasCurrent.hotelService
                      this.formValidate.hotelFacility = this.datasCurrent.hotelFacility
                      console.log(this.formValidate.hotelFacility)
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
                    width: '60px',
                    float: 'left'
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
    getListHotelByPage () {
      this.axios
        .get('http://localhost:8000/club/hotel/findAllByAttribute', {
          params: {
            currentPage: this.currentPages,
            pageCount: this.pageCount
          }
        })
        .then(response => {
          console.log(response.data)
          //   this.gid = response.data.gid;
          // 返回数据
          this.data1 = response.data
          // 取消加载状态
          this.loading = false
          // 给所有的记录条数赋值
          this.sumProduct = response.data.sumProduct
          this.datas = response.data
        })
    },
    search () {
      this.axios
        .get('http://localhost:8080/adminproject/searchHotel', {
          params: {
            currentPage: this.currentPage,
            pageCount: this.pageCount,
            hotel_location: this.searchs.hotel_location,
            hotel_name: this.searchs.hotel_name
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
    searchOrAll () {
      // 判断是否查询所有还是查询按条件查询
      if ((this.searchs.hotel_location == '' || this.searchs.hotel_location == null) && (this.searchs.hotel_name == '' || this.searchs.hotel_name == null)) {
        // 查询所有
        this.getListHotelByPage()
      } else {
        // 查询局部先将当期页改为1
        this.currentPage = 1
        this.search()
      }
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
          this.getListHotelByPage()
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
      this.axios.delete('http://localhost:8080/adminproject/removeHotel/' + this.delId, {})
        .then(response => {
          if (response.data == 'success') {
            this.cl()
            // 重新发送axios请求刷新页面
            this.getListHotelByPage()
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
      this.currentPage = currentPages
      console.log(this.currentPages)
      // 发送ajax请求调用判断是否是查询
      if ((this.searchs.hotel_location == '' || this.searchs.hotel_location == null) && (this.searchs.hotel_name == '' || this.searchs.hotel_name == null)) {
        this.getListHotelByPage()
      } else {
        this.search()
      }
    },
    // 改变显示条数
    changePageCount (pageCount) {
      // 给显示的记录条数赋值
      this.pageCount = pageCount
      // 发送ajax请求调用
      this.getListHotelByPage()
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
    this.getListHotelByPage()
  }
}
</script>
<style>
  .modal_left {
    width: 380px;
    height: 200px;
    float: left;
    /* 图片中的字体 */
    color: #3C3C3C;
    font-size: 14px;
    /* height:150px; */
  }

  .modal_right {
    /* width:230px; */
    width: 500px;
    height: 170px;
    float: right;
    font-size: 14px;
    color: #2F2F2F;
    /* height:500px; */
  }

  .model_img {
    width: 100%;
    border-radius: 10px;
    margin: 0 auto;
  }

  .ivu-modal-body {
    height: 603px;
  }

  .modal_right p {
    margin-top: 2px;
  }

  .vertical-center-modal {
     /*height:1800px;*/
  }
  .search {
    width: 170px;
  }
  .title_name{
    color: #535353b5;
  }
</style>
