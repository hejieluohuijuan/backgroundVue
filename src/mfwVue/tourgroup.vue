<template>
  <div>
    <row>
      <Drawer title="修改面板" v-model="value3" width="400" :mask-closable="false" :styles="styles">
        <Form :model="formItem" :label-width="80">
          <FormItem label="编号">
            <span>{{formData.travel_team_id}}</span>
          </FormItem>
          <FormItem label="标题描述">
            <Input
              v-model="formData.travel_team_title"
              type="textarea"
              :autosize="{minRows: 2,maxRows: 5}"
              placeholder="Enter something..."
            />
          </FormItem>
          <FormItem label="关键字">
            <Input v-model="formData.travel_team_keyword" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="性质">
            <Select v-model="formData.travel_team_type">
              <Option value="半自由行">半自由行</Option>
              <Option value="当地玩乐">当地玩乐</Option>
              <Option value="跟团游">跟团游</Option>
              <Option value="自由行">自由行</Option>
              <Option value="套餐">套餐</Option>
              <Option value="门票">门票</Option>
              <Option value="邮轮">邮轮</Option>
            </Select>
          </FormItem>
          <FormItem label="提供商家">
            <Input v-model="formData.travel_team_shoper" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="线路">
            <Input v-model="formData.travel_team_line" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="价格">
            <Input v-model="formData.travel_team_price" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="起始时间">
            <Row>
              <Col span="24">
                <DatePicker
                  type="date"
                  placeholder="Select date"
                  v-model="formData.travel_team_beginTime"
                ></DatePicker>
              </Col>
            </Row>
          </FormItem>
          <FormItem label="结束时间">
            <Row>
              <Col span="24">
                <DatePicker
                  type="date"
                  placeholder="Select date"
                  v-model="formData.travel_team_endTime"
                ></DatePicker>
              </Col>
            </Row>
          </FormItem>
          <FormItem label="起始地点">
            <Input v-model="formData.travel_team_origin" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="结束地点">
            <Input v-model="formData.travel_team_distance" placeholder="Enter something..."/>
          </FormItem>
          <FormItem>
            <Button type="ghost" @click="restUpdate">重置</Button>
          </FormItem>
          <FormItem>
            <hr>
          </FormItem>
        </Form>
        <div class="demo-drawer-footer">
          <Button style="margin-right: 8px" @click="value3 = false">Cancel</Button>
          <Button type="primary" @click="update">Submit</Button>
        </div>
      </Drawer>
    </row>
    <!-- showImg -->
    <Modal title="View Image" v-model="imgVisible">
      <img :src="imgSrc" v-if="imgVisible" style="width: 100%">
    </Modal>
    <!--end-adduser-->
    <Modal
      v-model="changeModal"
      ok-text="新增"
      cancel-text="退出"
      @on-ok="addUser"
      draggable
      scrollable
      title="旅行团新增面板"
    >
      <div>
        <!-- 我是新增模块 -->
        <Form :model="formItem" :label-width="80">
          <FormItem label="标题描述">
            <Input
              v-model="formItem.travel_team_title"
              type="textarea"
              :autosize="{minRows: 2,maxRows: 5}"
              placeholder="Enter something..."
            />
          </FormItem>
          <FormItem label="关键字">
            <Input v-model="formItem.travel_team_keyword" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="性质">
            <Select v-model="formItem.travel_team_type">
              <Option value="半自由行">半自由行</Option>
              <Option value="当地玩乐">当地玩乐</Option>
              <Option value="跟团游">跟团游</Option>
              <Option value="自由行">自由行</Option>
              <Option value="套餐">套餐</Option>
              <Option value="门票">门票</Option>
              <Option value="邮轮">邮轮</Option>
            </Select>
          </FormItem>
          <FormItem label="提供商家">
            <Input v-model="formItem.travel_team_shoper" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="线路">
            <Input v-model="formItem.travel_team_line" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="价格">
            <Input v-model="formItem.travel_team_price" placeholder="Enter something..."/>
          </FormItem>
          <FormItem label="起始时间">
            <Row>
              <Col span="24">
                <DatePicker
                  type="date"
                  placeholder="Select date"
                  v-model="formItem.travel_team_beginTime"
                ></DatePicker>
              </Col>
            </Row>
          </FormItem>
          <FormItem label="结束时间">
            <Row>
              <Col span="24">
                <DatePicker
                  type="date"
                  placeholder="Select date"
                  v-model="formItem.travel_team_endTime"
                ></DatePicker>
              </Col>
            </Row>
          </FormItem>
          <FormItem label="起始地点">
            <Input v-model="formItem.travel_team_origin" placeholder="Enter something..."/>
            <!-- <Select v-model="formItem.travel_team_origin">
              <Option value="beijing">New York</Option>
              <Option value="shanghai">London</Option>
              <Option value="shenzhen">Sydney</Option>
            </Select>-->
          </FormItem>
          <FormItem label="结束地点">
            <Input v-model="formItem.travel_team_distance" placeholder="Enter something..."/>
            <!-- <Select v-model="formItem.travel_team_distance">
              <Option value="beijing">New York</Option>
              <Option value="shanghai">London</Option>
              <Option value="shenzhen">Sydney</Option>
            </Select>-->
          </FormItem>
          <!-- <FormItem label="Slider">
            <Slider v-model="formItem.slider" range></Slider>
          </FormItem>-->
          <FormItem>
            <!-- <Button type="primary">Submit</Button> -->
            <Button type="ghost" @click="clearAdd">重置</Button>
          </FormItem>
        </Form>
      </div>
    </Modal>
    <!--end-changeuser-->
    <row class-name="bottom10" type="flex" align="middle">
      <Col span="6">
        <Button @click="handleSelectAll(true)">设置全选</Button>
        <Button @click="handleSelectAll(false)">取消全选</Button>
        <Button @click="changeModal=true" type="primary">新增</Button>
      </Col>
      <Col span="5" offset="13">
        <Input
          v-model="search"
          @on-search="getData()"
          search
          enter-button
          placeholder="Enter something..."
        />
      </Col>
    </row>
    <row class-name="bottom10">
      <Table :highlight-row="true" border ref="selection" :columns="columns" :data="data"></Table>
    </row>
    <row type="flex" justify="center" align="middle">
      <Col>
        <Page
          :total="total"
          :page-size="pageSize"
          :current="currentPage"
          show-total
          show-sizer
          @on-change="pageChange"
          @on-page-size-change="sizeChange"
          :page-size-opts="[5,10,15]"
        />
      </Col>
    </row>
  </div>
</template>

<style>
.bottom10 {
  margin-bottom: 10px;
}

.demo-drawer-footer {
  width: 100%;
  position: absolute;
  bottom: 0;
  left: 0;
  border-top: 1px solid #e8e8e8;
  padding: 10px 16px;
  text-align: right;
  background: #fff;
}

.demo-upload-list {
  display: inline-block;
  width: 60px;
  height: 60px;
  text-align: center;
  line-height: 60px;
  border: 1px solid transparent;
  border-radius: 4px;
  overflow: hidden;
  background: #fff;
  position: relative;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2);
  margin-right: 4px;
}
.demo-upload-list img {
  width: 100%;
  height: 100%;
}
.demo-upload-list-cover {
  display: none;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.6);
}
.demo-upload-list:hover .demo-upload-list-cover {
  display: block;
}
.demo-upload-list-cover i {
  color: #fff;
  font-size: 20px;
  cursor: pointer;
  margin: 0 2px;
}
</style>

<script>
export default {
  components: {

  },
  data () {
    return {
      formItem: {
        travel_team_img: '',
        travel_team_title: '',
        travel_team_keyword: '',
        travel_team_type: '',
        travel_team_shoper: '',
        travel_team_line: '',
        travel_team_price: '',
        travel_team_beginTime: '',
        travel_team_endTime: '',
        travel_team_origin: '',
        travel_team_distance: ''
        // slider: [20, 50]
      },

      changeModal: false,
      value3: false,
      styles: {
        height: 'calc(100% - 55px)',
        overflow: 'auto',
        paddingBottom: '53px',
        position: 'static'
      },
      formData: {
        travel_team_img: '',
        travel_team_id: '',
        travel_team_title: '',
        travel_team_keyword: '',
        travel_team_type: '',
        travel_team_shoper: '',
        travel_team_line: '',
        travel_team_price: '',
        travel_team_beginTime: '',
        travel_team_endTime: '',
        travel_team_origin: '',
        travel_team_distance: '',
        travel_team_peoples: 0,
        travel_team_collects: 0
      },

      imgVisible: false,
      imgSrc: '',
      changeIndex: -1,
      Message: '',
      search: '',
      offSet: 0,
      pageSize: 5,
      currentPage: 1,
      total: 0,
      columns: [
        {
          type: 'selection',
          width: 60,
          align: 'center'
        },
        {
          title: '旅行团编号',
          key: 'travel_team_id'
        },
        {
          title: '效果图',
          key: 'travel_team_img',
          render: (h, params) => {
            return h(
              'div',
              {
                class: {
                  'demo-upload-list': true
                }
              },
              [
                h('img', {
                  attrs: {
                    src: params.row.travel_team_img
                  }
                }),
                h(
                  'div',
                  {
                    class: {
                      'demo-upload-list-cover': true
                    }
                  },
                  [
                    h('Icon', {
                      props: {
                        type: 'ios-eye-outline'
                      },
                      on: {
                        click: () => {
                          this.updateImg(params.row.travel_team_img)
                        }
                      }
                    })
                  ]
                )
              ]
            )
          }
        },
        {
          title: '旅行团的标题描述',
          key: 'travel_team_title'
        },
        {
          title: '旅行团的关键字',
          key: 'travel_team_keyword'
        },
        {
          title: '旅行团的性质',
          key: 'travel_team_type'
        },
        {
          title: '本次旅行团的提供商家',
          key: 'travel_team_shoper'
        },
        {
          title: '本次旅行团的线路',
          key: 'travel_team_line'
        },
        {
          title: '本次旅行团的价格',
          key: 'travel_team_price'
        },
        {
          title: '本次旅行团的起始时间',
          key: 'travel_team_beginTime'
        },
        {
          title: '本次旅行团的结束时间',
          key: 'travel_team_endTime'
        },
        {
          title: '本次旅行团的起始地点',
          key: 'travel_team_origin'
        },
        {
          title: '本次旅行团的结束地点',
          key: 'travel_team_distance'
        },
        {
          title: '本次旅行团的销售量',
          key: 'travel_team_peoples'
        },
        {
          title: '本次旅行团的收藏量',
          key: 'travel_team_collects'
        },
        {
          title: '操作',
          key: 'action',
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
                    marginRight: '5px'
                  },
                  on: {
                    click: () => {
                      this.show(params.index)
                    }
                  }
                },
                '修改'
              ),
              h(
                'Button',
                {
                  props: {
                    type: 'error',
                    size: 'small'
                  },
                  on: {
                    click: () => {
                      this.remove(params.index)
                    }
                  }
                },
                '删除'
              )
            ])
          }
        }
      ],
      data: []
    }
  },
  methods: {
    handleSelectAll (status) {
      this.$refs.selection.selectAll(status)
    },
    myFormatDate (date) {
      date.formData
      return date.getFullYear() + '-' + date.getMonth() + '-' + date.getDate()
    },
    updateImg (src) {
      this.imgSrc = src
      this.imgVisible = true
    },
    clearAdd () {
      // console.log('重置');
      this.formItem.travel_team_title = ''
      this.formItem.travel_team_keyword = ''
      this.formItem.travel_team_type = ''
      this.formItem.travel_team_shoper = ''
      this.formItem.travel_team_line = ''
      this.formItem.travel_team_price = ''
      this.formItem.travel_team_beginTime = ''
      this.formItem.travel_team_endTime = ''
      this.formItem.travel_team_origin = ''
      this.formItem.travel_team_distance = ''
    },
    addUser () {
      // console.log("发送添加请求");

      var qs = require('qs')
      var mydata = qs.stringify({
        travel_team_title: this.formItem.travel_team_title,
        travel_team_keyword: this.formItem.travel_team_keyword,
        travel_team_type: this.formItem.travel_team_type,
        travel_team_shoper: this.formItem.travel_team_shoper,
        travel_team_line: this.formItem.travel_team_line,
        travel_team_price: this.formItem.travel_team_price,
        travel_team_beginTime: this.formItem.travel_team_beginTime.toLocaleDateString(),
        travel_team_endTime: this.formItem.travel_team_endTime.toLocaleDateString(),
        travel_team_origin: this.formItem.travel_team_origin,
        travel_team_distance: this.formItem.travel_team_distance
      })
      this.axios
        .post('http://localhost:8080/adminproject/saveTravelTeam', mydata)
        .then(response => {
          // console.log(response);
          if (response.data) {
            this.Message = '添加成功!'
            this.success(false)
            this.getData()
          } else {
            this.Message = '添加失败!'
            this.error(false)
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    show (index) {
      this.value3 = true
      this.changeIndex = index
      this.restUpdate()
    },
    restUpdate () {
      var i = this.changeIndex
      this.formData.travel_team_img = this.data[i].travel_team_img
      this.formData.travel_team_id = this.data[i].travel_team_id
      this.formData.travel_team_title = this.data[i].travel_team_title
      this.formData.travel_team_keyword = this.data[i].travel_team_keyword
      this.formData.travel_team_type = this.data[i].travel_team_type
      this.formData.travel_team_shoper = this.data[i].travel_team_shoper
      this.formData.travel_team_line = this.data[i].travel_team_line
      this.formData.travel_team_price = this.data[i].travel_team_price
      this.formData.travel_team_beginTime = this.data[i].travel_team_beginTime
      this.formData.travel_team_endTime = this.data[i].travel_team_endTime
      this.formData.travel_team_origin = this.data[i].travel_team_origin
      this.formData.travel_team_distance = this.data[i].travel_team_distance
      this.formData.travel_team_peoples = this.data[i].travel_team_peoples
      this.formData.travel_team_collects = this.data[i].travel_team_collects
      // console.log("修改" + this.data[i].travel_team_id);
    },
    update () {
      this.value3 = false
      var qs = require('qs')
      var newData = qs.stringify({
        travel_team_img: this.formData.travel_team_img,
        travel_team_id: this.formData.travel_team_id,
        travel_team_title: this.formData.travel_team_title,
        travel_team_keyword: this.formData.travel_team_keyword,
        travel_team_type: this.formData.travel_team_type,
        travel_team_shoper: this.formData.travel_team_shoper,
        travel_team_line: this.formData.travel_team_line,
        travel_team_price: this.formData.travel_team_price,
        travel_team_beginTime: this.formData.travel_team_beginTime.toLocaleDateString(),
        travel_team_endTime: this.formData.travel_team_endTime.toLocaleDateString(),
        travel_team_origin: this.formData.travel_team_origin,
        travel_team_distance: this.formData.travel_team_distance,
        travel_team_peoples: this.formData.travel_team_peoples,
        travel_team_collects: this.formData.travel_team_collects
      })
      this.axios
        .post('http://localhost:8080/adminproject/updateTravelTeam', newData)
        .then(response => {
          // console.log(response);
          if (response.data) {
            this.Message = '修改成功!'
            this.success(false)
            this.getData()
          } else {
            this.Message = '修改失败!'
            this.error(false)
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
    remove (index) {
      // console.log(this.data[index].travel_team_id);
      this.axios
        .delete(
          'http://localhost:8080/adminproject/removeTravelTeam/' +
            this.data[index].travel_team_id
        )
        .then(response => {
          // console.log(response);
          if (response.data) {
            this.Message = '删除成功!'
            this.success(false)
            this.getData()
          } else {
            this.Message = '删除失败!'
            this.error(false)
          }
        })
        .catch(error => {
          console.log(error)
        })
      this.data.splice(index, 1)
      // console.log("删除" + this.data[index].travel_team_id);
    },
    pageChange (currentPage) {
      this.currentPage = currentPage
      this.updateOffSet()
      this.getData()
    },
    sizeChange (pageSize) {
      this.pageSize = pageSize
      this.updateOffSet()
      this.getData()
    },
    updateOffSet () {
      this.offSet = (this.currentPage - 1) * this.pageSize
      // console.log(this.offSet);
    },
    getData () {
      // console.log("给我数据");
      this.axios
        .get('http://localhost:8080/adminproject/listTravelTeams', {
          params: {
            search: this.search,
            offset: this.offSet,
            pageSize: this.pageSize
          }
        })
        .then(response => {
          // console.log(response.data);
          this.total = response.data.total
          this.data = response.data.data
        })
        .catch(error => {
          console.log(error)
        })
    },
    success (nodesc) {
      // console.log(nodesc);
      this.$Notice.success({
        title: '操作通知',
        desc: nodesc ? '' : this.Message
      })
    },
    error (nodesc) {
      this.$Notice.error({
        title: '操作通知',
        desc: nodesc ? '' : this.Message
      })
    }
  },

  mounted () {
    this.getData()
  }
}
</script>
