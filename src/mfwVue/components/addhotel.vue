<template>
    <div>
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
         <FormItem label="入住时间" prop="formValidate.hotel_intime" width="160px">
             <Input  placeholder="入住时间" v-model="formValidate.hotel_intime" style="width:150px;"/>
        </FormItem>
         <FormItem label="时间退房" prop="formValidate.hotel_outtime" style="width:160px;">
              <Input  placeholder="退房时间" v-model="formValidate.hotel_outtime" style="width:150px;"/>
        </FormItem>

        <FormItem label="建造时间" prop="formValidate.hotel_buildtime">
             <Input  placeholder="建造时间年份" v-model="formValidate.hotel_buildtime" style="width:150px;"/>
        </FormItem>
         <FormItem label="装修时间" prop="formValidate.hotel_rebuildtime">
              <Input  placeholder="装修时间年份" v-model="formValidate.hotel_rebuildtime" style="width:150px;"/>
        </FormItem>
         <FormItem label="酒店规模" prop="formValidate.hotel_rooms">
            <Input  placeholder="酒店规模" v-model="formValidate.hotel_rooms"/>
        </FormItem>

         <FormItem label="主要设施" prop="formValidate.main_facility">
            <CheckboxGroup v-model="formValidate.main_facility">
                <!--遍历的应该是所有，对应的会默认选中-->
                 <Checkbox label="免费wifi"></Checkbox>
                 <Checkbox label="电梯"></Checkbox>
                 <Checkbox label="餐厅"></Checkbox>
                 <Checkbox label="免费停车场"></Checkbox>
                 <Checkbox label="行李寄存"></Checkbox>
                 <Checkbox label="24小时服务"></Checkbox>
                 <Checkbox label="热水壶"></Checkbox>
                 <Checkbox label="吹风机"></Checkbox>
            </CheckboxGroup>
        </FormItem>

         <FormItem label="酒店服务" prop=".formValidatehotel_service">
                 <CheckboxGroup v-model="formValidate.hotel_service">
                  <Checkbox label="接机服务"></Checkbox>
                 <Checkbox label="代客泊车"></Checkbox>
                 <Checkbox label="自行车租赁"></Checkbox>
                 <Checkbox label="快速入住服务"></Checkbox>
                 <Checkbox label="叫车服务"></Checkbox>
                 <Checkbox label="洗衣服务"></Checkbox>
            </CheckboxGroup>
        </FormItem>

         <FormItem label="酒店设施" prop="formValidate.hotel_facility">
            <CheckboxGroup v-model="formValidate.hotel_facility">
                  <Checkbox label="免费wifi"></Checkbox>
                  <Checkbox label="餐厅"></Checkbox>
                  <Checkbox label="行李寄存"></Checkbox>
                  <Checkbox label="热水壶"></Checkbox>
                  <Checkbox label="吹风机"></Checkbox>
            </CheckboxGroup>
        </FormItem>
        </FormItem>
         <FormItem label="房间设施" prop="formValidate.room_facility">
            <CheckboxGroup v-model="formValidate.room_facility">
                <Checkbox label="24小时热水"></Checkbox>
                <Checkbox label="阳台/露台"></Checkbox>
                <Checkbox label="电视"></Checkbox>
                <Checkbox label="淋浴设施"></Checkbox>
                <Checkbox label="公共卫浴"></Checkbox>
                 <Checkbox label="独立卫浴间"></Checkbox>
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
    </div>
</template>
<script>
export default{
  data () {
    return {
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
      }
    }
  },
  methods: {
    handleSubmit (name) {
      this.$refs[name].validate((valid) => {
        if (valid) {
          // this.$Message.success('Success!');
          this.allHotel()
        } else {
          // this.$Message.error('Fail!');
        }
      })
    },
    handleReset (name) {
      this.$refs[name].resetFields()
    },
    // 添加
    allHotel () {
      let config = {
        headers: {
          'Content-Type': 'multipart/form-data'
          // "Accept":"*/*",
          // "Access-Control-Allow-Methods":" . (isset($_GET['allow_method']) ? $_GET['allow_method'] : 'OPTIONS')"
        }
      }
      this.axios.post('http://localhost:8080/adminproject/addHotel', {
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
        this.$Message.success('添加成功!')
      } else {
        this.$Message.error('服务器正忙')
      }
    }
  }
}
</script>
<style>
</style>
