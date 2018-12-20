<template>
  <div>

    <Form ref="formCustom" :model="formCustom"  :label-width="60" inline>
        <FormItem label="订单编号" prop="oid">
            <Input type="text" v-model="formCustom.oid"></Input>
        </FormItem>

        <FormItem label="用户编号" prop="uid">
            <Input type="text" v-model="formCustom.uid"></Input>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="handleFormSubmit('formCustom')">Submit</Button>
        </FormItem>
    </Form>

    <Button @click="modal=true;formValidate={};formTitle='添加订单'">添加订单</Button>
    <Table border :columns="columns" :data="data"></Table>

    <Page
      :total="totalCount"
      :page-size="pageSize"
      @on-change="onChangePage"
      @on-page-size-change="onPageSizeChange"
      :page-size-opts="[5,10,15,20]"
      show-sizer
      prev-text="Previous"
      next-text="Next"
    />
    <Modal v-model="modal" :title="formTitle" @on-cancel="cancel">
      <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
        <FormItem label="用户ID" prop="userId">
          <Input v-model="formValidate.userId" placeholder></Input>
        </FormItem>
        <FormItem label="商品ID" prop="productId">
          <Input v-model="formValidate.productId" placeholder></Input>
        </FormItem>
        <FormItem label="商品价格" prop="price">
          <Input v-model="formValidate.price" placeholder></Input>
        </FormItem>
        <FormItem label="商品数量" prop="counts">
          <Input v-model="formValidate.counts" placeholder></Input>
        </FormItem>
        <FormItem label="购买时间">
          <Row>
            <Col span="11">
              <FormItem prop="buytime">
                <DatePicker
                  type="datetime"
                  placeholder="Select date"
                  v-model="formValidate.buytime"
                ></DatePicker>
              </FormItem>
            </Col>
          </Row>
        </FormItem>
        <FormItem label="支付时间">
          <Row>
            <Col span="11">
              <FormItem prop="paytime">
                <DatePicker
                  type="datetime"
                  placeholder="Select date"
                  v-model="formValidate.paytime"
                ></DatePicker>
              </FormItem>
            </Col>
          </Row>
        </FormItem>
        <FormItem label="订单状态" prop="state">
          <RadioGroup v-model="formValidate.state">
            <Radio label="0">未支付</Radio>
            <Radio label="1">已支付</Radio>
          </RadioGroup>
        </FormItem>

        <FormItem>
          <Button type="primary" @click="handleSubmit('formValidate')">Submit</Button>
          <Button @click="handleReset('formValidate')" style="margin-left: 8px">Reset</Button>
        </FormItem>
      </Form>
      <div slot="footer"></div>
    </Modal>
  </div>
</template>
<script>
export default {
  data () {
    return {
      formCustom: {
        oid: '',
        uid: ''
      },
      oldDate: '',
      formTitle: '',
      formValidate: {
        orderId: '',
        productId: '',
        userId: '',
        title: '',
        counts: '',
        price: '',
        total: '',
        state: '',
        buytime: '',
        paytime: ''

      },
      ruleValidate: {
        // username: [
        //   {
        //     required: true,
        //     message: "The username cannot be empty",
        //     trigger: "blur"
        //   }
        // ],
        // email: [
        //   {
        //     required: true,
        //     message: "Mailbox cannot be empty",
        //     trigger: "blur"
        //   },
        //   { type: "email", message: "Incorrect email format", trigger: "blur" }
        // ],
        // password: [
        //   {
        //     required: true,
        //     message: "密码长度应为6-12",
        //     trigger: "change",
        //     min: 6,
        //     max: 12
        //   }
        // ],
        // gender: [
        //   { required: true, message: "Please select gender", trigger: "change" }
        // ],
        // interest: [
        //   {
        //     required: true,
        //     type: "array",
        //     min: 1,
        //     message: "Choose at least one hobby",
        //     trigger: "change"
        //   },
        //   {
        //     type: "array",
        //     max: 2,
        //     message: "Choose two hobbies at best",
        //     trigger: "change"
        //   }
        // ],
        // date: [
        //   {
        //     required: true,
        //     type: "date",
        //     message: "Please select the date",
        //     trigger: "change"
        //   }
        // ]
        // desc: [
        //   {
        //     required: true,
        //     message: "Please enter a personal introduction",
        //     trigger: "blur"
        //   },
        //   {
        //     type: "string",
        //     min: 20,
        //     message: "Introduce no less than 20 words",
        //     trigger: "blur"
        //   }
        // ]
      },
      modal: false,
      columns: [
        {
          title: '订单编号',
          key: 'orderId'
        },
        {
          title: '用户编号',
          key: 'userId'
        },
        {
          title: '商品编号',
          key: 'productId'
        },
        {
          title: '数量',
          key: 'counts'
        },
        {
          title: '单价',
          key: 'price'
        },
        {
          title: '总价',
          key: 'total'
        },
        {
          title: '订单状态',
          key: 'state'
        },
        {
          title: '购买时间',
          key: 'buytime'
        },
        // {
        //   title: "支付时间",
        //   key: "paytime"
        // },

        {
          title: 'Action',
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
                      this.formTitle = '编辑订单'
                      this.edit(params.row.orderId)
                    }
                  }
                },
                'Edit'
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
                      console.log(params.row)
                      this.remove(params.row.orderId)
                    }
                  }
                },
                'Delete'
              )
            ])
          }
        }
      ],
      data: [],
      totalCount: 5,
      pageSize: 5,
      pageIndex: 2
    }
  },
  methods: {
    handleFormSubmit (name) {
      console.log('1111')
      this.getData()
    },

    edit (oid) {
      this.axios({
        url: 'http://localhost:8080/adminproject/order/' + oid,
        method: 'get'
      }).then(res => {
        console.log(res.data)

        this.formValidate = res.data
        this.formValidate.state += ''
        this.formValidate.buytime = new Date(this.formValidate.buytime)
        if (this.formValidate.paytime) {
          this.formValidate.paytime = new Date(this.formValidate.paytime)
        }

        this.modal = true
        this.getData()
        console.log('======================================')
      })
    },
    remove (oid) {
      console.log(oid)
      this.$Modal.confirm({
        title: 'Title',
        content: '<p>确认删除?</p>',
        onOk: () => {
          this.axios({
            // params: {
            //   orderId: oid
            // },
            url: 'http://localhost:8080/adminproject/order/' + oid,
            method: 'delete'
          }).then(res => {
            this.$Message.info('删除成功')
            this.getData()
          })
        },
        onCancel: () => {
          this.$Message.info('Clicked cancel')
        }
      })

      //   this.data.splice(index, 1);
    },
    getData () {
      this.axios({
        url: 'http://localhost:8080/adminproject/order',
        method: 'get',
        params: {
          userId: this.formCustom.uid,
          orderId: this.formCustom.oid,
          pageIndex: this.pageIndex,
          pageSize: this.pageSize
        }
      }).then(res => {
        console.log(res.data)
        this.totalCount = res.data.totalCount
        this.pageSize = res.data.pageSize
        console.log(this.totalCount)
        this.data = res.data.orders

        for (var i = 0; i < this.data.length; i++) {
          this.data[i].buytime = new Date(this.data[i].buytime)
          // console.log(this.data[i].buytime)
        }
        //
        // this.totalCount = res.data.userVO.totalCount;
        // this.pageSize = res.data.userVO.pageSize;
      })
    },
    onChangePage (page) {
      this.pageIndex = page
      this.getData()
      console.log(this.pageIndex)
    },
    onPageSizeChange (pageSize) {
      this.pageSize = pageSize
      this.getData()
      console.log(pageSize, this.pageSize)
    },

    cancel () {
      this.$Message.info('cancel')
      this.getData()
    },
    handleSubmit (name) {
      console.log(this.formValidate)
      this.$refs[name].validate(valid => {
        if (valid) {
          if (this.formValidate.orderId) {
            this.$http
              .put(
                'http://localhost:8080/adminproject/order',
                this.formValidate
              )
              .then(
                res => {
                  // success callback
                  this.getData()
                  this.$Message.info('修改成功')
                }
                // err => {
                //   this.$Message.info('修改失败')
                // }
              )
          } else {
            this.$http
              .post(
                'http://localhost:8080/adminproject/order',
                this.formValidate
              )
              .then(
                res => {
                  // success callback
                  this.$Message.info('添加成功')
                  this.getData()
                }
                // err => {
                //   this.$Message.info('添加失败')
                // }
              )
          }
          this.modal = false
        } else {
          this.$Message.error('Fail!')
        }
      })
    },
    handleReset (name) {
      this.$refs[name].resetFields()
    }
  },
  mounted () {
    this.getData()
  }
}
</script>
