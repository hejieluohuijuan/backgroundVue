<template>
  <div id="allUser">
    <div class="nav-top"></div>
    <div class="nav-menu">
      <input type="text" placeholder="搜索" class="search-input" @keyup.enter="searchByKey">
    </div>

    <Table :columns="userColumns" :data="users" class="table table-border"></Table>
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
                  src: 'https://i.loli.net/2017/08/21/599a521472424.jpg'
                },
                style: {},
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
          title: '激活',
          key: 'operate',
          align: 'center',
          render: (h, params) => {
            return h('div', [
              h('i-switch', {
                props: {
                  // type: 'primary',
                  size: 'small'
                },
                style: {
                  // marginRight: '5px'
                }
                // on: {
                //   click: () => {
                //     this.show(params.index)
                //   }
                // }
              }, 'View')

            ])
          }
        }
      ]
    }
  },
  methods: {
    searchByKey (event) {
      this.axios.get(this.url + 'admin/search', { params: { nickname: event.target.value } }).then((resp) => {
        this.users = resp.data
        this.users[0].createTime = new Date(this.users[0].createTime)
        console.log(this.users)
      }).catch((resp) => {
        console.log(resp)
      })
    },
    getAuthor (id) {
      this.axios.get(this.url + 'admin/search', { params: { userInfoId: id } }).then((resp) => {
        this.author = resp.data
      })
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
      })
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
