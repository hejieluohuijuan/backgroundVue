<template>
  <div id="allUser">
    <div class="nav-top"></div>
    <div class="nav-menu">
      <input type="text" placeholder="搜索" class="search-input" @keyup.enter="searchByKey">
    </div>

    <Table :columns="userColumns" :data="users" class="table table-border">
      <template slot-scope="{ row, index }" slot="action">
        <i-switch size="small" v-model="users[index].status" @on-change="editUserInfo(index)">
        </i-switch>
      </template>
      <!--<template slot-scope="{ row, index }" slot="createTime">-->
        <!--<span v-model=""></span>-->
      <!--</template>-->
    </Table>
    <div style="margin: 10px;overflow: hidden">
      <div style="float: right;">
        <Page :total="userCount" :current="pageIndex" :page-size="pageSize"
              @on-change="onChange" @on-page-size-change="onPageSizeChange" show-sizer></Page>
      </div>
    </div>

  </div>
</template>
<script>
export default {
  name: 'allUser',
  data () {
    return {
      users: [],
      author: [],
      userCount: 1,
      pageSize: 20,
      pageIndex: 1,
      url: 'http://localhost:8080/adminproject/user/',
      userColumns: [
        {
          title: '头像',
          key: 'avatar',
          fixed: 'left',
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h('avatar', {
                props: {
                  src: 'https://img2.woyaogexing.com/2018/12/20/062b9adfa560e8da!400x400_big.jpg',
                  shape:'square',
                },
                style: {
                  width: '50px',
                  height: '40px',
                  margin: '0px',
                  borderRadius: '5px' },
                on: {}
              }, 'view')
            ])
          }
          // white: 5,
        }, {
          title: '昵称',
          key: 'nickname',
          align: 'center'
        },
        {
          title: '性别',
          key: 'gender',
          align: 'center'
        },
        {
          title: '地区',
          key: 'area',
          align: 'center'
        },
        {
          title: '国家',
          key: 'country',
          align: 'center'
        },
        {
          title: '创建时间',
          key: 'createTime',
          align: 'center'
        },
        {
          title: '授权登录',
          key: 'detail',
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h('Icon', {
                props: {
                  type: 'ios-chatbubbles',
                  size: '17',
                  color: 'green'
                },
                style: {
                  marginRight: '5px'
                }
                // on: {
                //   click: () => {
                //     this.show(params.index)
                //   }
                // }
              }, 'View'),
              h('Icon', {
                props: {
                  type: 'ios-mail',
                  size: '20',
                  color: 'green'
                },
                style: {
                  marginRight: '5px'
                }
                // on: {
                //   click: () => {
                //     this.show(params.index)
                //   }
                // }
              }, 'View'),
              h('Icon', {
                props: {
                  type: 'md-phone-portrait',
                  size: '20',
                  color: 'green'
                },
                style: {
                  marginRight: '5px'
                }
                // on: {
                //   click: () => {
                //     this.show(params.index)
                //   }
                // }
              }, 'View')

            ])
          }
        },
        {
          title: '状态设置',
          align: 'center',
          slot: 'action'
        }
      ]
    }
  },
  methods: {
    searchByKey (event) {
      this.axios.get(this.url + 'admin/search', { params: { nickname: event.target.value } }).then((resp) => {
        this.users = resp.data
        this.parseUserDate()
        console.log(this.users)
      }).catch((resp) => {
        console.log(resp)
      })
    },
    getAuthor (id) {
      this.axios.get(this.url + 'admin/search', { params: { userInfoId: id } }).then((resp) => {
        this.author = resp.data
        this.parseUserDate()
      })
    },
    editUserInfo (index) {
      console.log(index)
      this.axios.put(this.url + 'admin',
        this.users[index]
      ).then().catch()
    },
    onChange (index) {
      this.pageIndex = index
      this.listUsers()
    },
    onPageSizeChange (size) {
      this.pageSize = size
      this.listUsers()
    },
    listUsers () {
      this.axios.get(this.url + 'admin', {
        params: {
          pageIndex: this.pageIndex,
          pageSize: this.pageSize
        }
      }).then((resp) => {
        this.users = resp.data
        this.parseUserDate()
      })
    },
    parseUserDate () {
      for (var i = 0; i < this.users.length; i++) {
        this.users[i].createTime = this.parseDate(this.users[i].createTime);
        this.users[i].gender = this.users[i].gender? '男':'女';

      }
    },
    parseDate (cellval) {
      var dateVal = cellval + ''
      if (cellval != null) {
        var date = new Date(parseInt(dateVal.replace('/Date(', '').replace(')/', ''), 10))
        var month = date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1
        var currentDate = date.getDate() < 10 ? '0' + date.getDate() : date.getDate()

        // var hours = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
        // var minutes = date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
        // var seconds = date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();

        return date.getFullYear() + '-' + month + '-' + currentDate
      }
    }
  },
  created () {
    this.axios.get(this.url + 'admin', {
      params: {
        pageIndex: this.pageIndex,
        pageSize: this.pageSize
      }
    }).then((resp) => {
      this.users = resp.data
      this.parseUserDate()
    })
    this.axios.get(this.url + 'admin/count').then((resp) => {
      this.userCount = resp.data
    })
  }
}

</script>
<style scoped>
  .nav-menu {
    height: 35px;
    background-color: #000c17;
  }

  .search-input {
    height: 20px;
    float: right;
    border: none;
    margin: 7px 100px auto;
  }

</style>
