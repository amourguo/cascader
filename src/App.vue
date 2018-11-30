<template>
  <div id="app">
    <!-- router-view/ -->
    <div class="container">
      <h2>新增片区</h2>
      <Cascader
        :districts="districts"
        @cascader-submit="handleSubmit"
        @cascader-cancel="handleCancel"
      />
      <h4 class="after-submit">{{ submitedData }}</h4>
    </div>
    <div class="instruction">
      <hr />
      <h4>依赖模块：vue-awesome</h4>
      <p>$ npm i --save vue-awesome</p>
      <p>"dependencies": { "vue-awesome": "^3.2.0" }</p>
      <h4>输入（props） districts: {Array} [ {Object}, ... ]</h4>
      <p>&lt;Cascader :districts="[...]" /&gt;</p>
      <textarea>
[
  {
    'hbType': '1',
    'hbParentId': '0',
    'children': [{
      'hbType': '2',
      'hbParentId': '2',
      'children': [{
        'hbType': '3',
        'hbParentId': '4',
        'hbRemark': '',
        'name': '片区BBB',
        'value': '6'
      }],
      'hbRemark': '',
      'name': '片区BB',
      'value': '4'
    }],
    'hbRemark': '',
    'name': '片区B',
    'value': '2'
  }, {
    'hbType': '0',
    'hbParentId': '0',
    'children': [{
      'hbType': '1',
      'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
      'children': [{
        'hbType': '2',
        'hbParentId': '5f42758d-cfdb-4816-acce-be9d497486ec',
        'hbRemark': '3级',
        'name': '吃饭',
        'value': 'd72f45ff-0ec8-4bd9-bf64-a1159c599e00'
      }],
      'hbRemark': '2级',
      'name': '打算',
      'value': '5f42758d-cfdb-4816-acce-be9d497486ec'
    }, {
      'hbType': '1',
      'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
      'children': [{
        'hbType': '2',
        'hbParentId': '77ce8550-8f8b-411c-b89e-c675241064f2',
        'hbRemark': '3级',
        'name': '今天新增三级',
        'value': 'd8943ab4-4bca-49e0-9ffb-17e93c0ee984'
      }],
      'hbRemark': '2级',
      'name': '今天新增二级',
      'value': '77ce8550-8f8b-411c-b89e-c675241064f2'
    }, {
      'hbType': '1',
      'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
      'hbRemark': '2级',
      'name': '大龙山',
      'value': '81fcb3ef-020a-4b2e-9b8e-b146ad18bb66'
    }, {
      'hbType': '1',
      'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
      'hbRemark': '2级',
      'name': '冉家坝',
      'value': 'b4f86107-9833-4eaa-aa66-da6157b681b9'
    }, {
      'hbType': '1',
      'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
      'children': [{
        'hbType': '2',
        'hbParentId': 'f6b8174b-c691-4650-b60f-d85488ee86cd',
        'hbRemark': '3级',
        'name': 'AAA',
        'value': '15e6baac-6e92-40b0-b955-77083e5ba06e'
      }],
      'hbRemark': '2级',
      'name': 'AA',
      'value': 'f6b8174b-c691-4650-b60f-d85488ee86cd'
    }],
    'hbRemark': '1级',
    'name': '爱车',
    'value': '49dd73da-b3e2-4a86-9b24-438f9b60f628'
  }, {
    'hbType': '0',
    'hbParentId': '0',
    'children': [{
      'hbType': '1',
      'hbParentId': '5471d0fc-4d42-493b-8875-f2ea71613658',
      'children': [{
        'hbType': '2',
        'hbParentId': '49a36108-124e-4aec-b075-e19dd93461ba',
        'hbRemark': '3级',
        'name': '二大爷',
        'value': 'e845a902-cd32-4096-9fc0-c916089f37f6'
      }],
      'hbRemark': '2级',
      'name': '我去',
      'value': '49a36108-124e-4aec-b075-e19dd93461ba'
    }, {
      'hbType': '1',
      'hbParentId': '5471d0fc-4d42-493b-8875-f2ea71613658',
      'children': [{
        'hbType': '2',
        'hbParentId': '8ec1ade7-041e-482d-a689-42a04192c37b',
        'hbRemark': '3级',
        'name': '吃仨',
        'value': 'c0d65f51-e081-4d32-9849-a485e9784fff'
      }],
      'hbRemark': '2级',
      'name': '感受',
      'value': '8ec1ade7-041e-482d-a689-42a04192c37b'
    }],
    'hbRemark': '1级',
    'name': '打野',
    'value': '5471d0fc-4d42-493b-8875-f2ea71613658'
  }, {
    'hbType': '0',
    'hbParentId': '0',
    'children': [{
      'hbType': '1',
      'hbParentId': 'b3c1d40d-c869-4740-b459-077b7ebf2dc3',
      'children': [{
        'hbType': '2',
        'hbParentId': '7e8bb226-853f-48d1-aa37-92fd44ef371c',
        'hbRemark': '3级',
        'name': '锦里',
        'value': 'edf835b0-ef66-41e4-b382-5536bb1ee84e'
      }],
      'hbRemark': '2级',
      'name': '成都',
      'value': '7e8bb226-853f-48d1-aa37-92fd44ef371c'
    }],
    'hbRemark': '1级',
    'name': '四川',
    'value': 'b3c1d40d-c869-4740-b459-077b7ebf2dc3'
  }, {
    'hbType': '0',
    'hbParentId': '0',
    'children': [{
      'hbType': '1',
      'hbParentId': 'e38c6167-89d9-409c-a2c0-8fb2f7e52a90',
      'children': [{
        'hbType': '2',
        'hbParentId': 'da4ad947-ca97-404d-8d99-e049986dc096',
        'hbRemark': '3级',
        'name': '赵信',
        'value': 'e7d502a2-b30a-4a46-b823-b34406e9a616'
      }],
      'hbRemark': '2级',
      'name': '盲僧',
      'value': 'da4ad947-ca97-404d-8d99-e049986dc096'
    }],
    'hbRemark': '1级',
    'name': '蒙多',
    'value': 'e38c6167-89d9-409c-a2c0-8fb2f7e52a90'
  }
]
      </textarea>
      <h4>输出：emit事件：'cascader-submit'</h4>
      <p>事件携带参数 {Array} [ {Object}, ... ]</p>
      <p>&lt;Cascader @cascader-submit="{Function} eventHandler" @cascader-cancel="{Function} eventHandler" /&gt;</p>
      <textarea>
[
   {
      "hbType":"0",
      "hbParentId":"0",
      "hbRemark":"1级",
      "name":"爱车",
      "value":"49dd73da-b3e2-4a86-9b24-438f9b60f628",
      "children":[
         {
            "hbType":"1",
            "hbParentId":"49dd73da-b3e2-4a86-9b24-438f9b60f628",
            "hbRemark":"2级",
            "name":"今天新增二级",
            "value":"77ce8550-8f8b-411c-b89e-c675241064f2",
            "children":[
               {
                  "name":"今天新增三级x",
                  "children":[]
               }
            ]
         }
      ]
   }
]
      </textarea>
    </div>
  </div>
</template>

<script>
import Cascader from './components/Cascader'

export default {
  name: 'App',
  data () {
    return {
      districts: [{
        'hbType': '1',
        'hbParentId': '0',
        'children': [{
          'hbType': '2',
          'hbParentId': '2',
          'children': [{
            'hbType': '3',
            'hbParentId': '4',
            'hbRemark': '',
            'name': '片区BBB',
            'value': '6'
          }],
          'hbRemark': '',
          'name': '片区BB',
          'value': '4'
        }],
        'hbRemark': '',
        'name': '片区B',
        'value': '2'
      }, {
        'hbType': '0',
        'hbParentId': '0',
        'children': [{
          'hbType': '1',
          'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
          'children': [{
            'hbType': '2',
            'hbParentId': '5f42758d-cfdb-4816-acce-be9d497486ec',
            'hbRemark': '3级',
            'name': '吃饭',
            'value': 'd72f45ff-0ec8-4bd9-bf64-a1159c599e00'
          }],
          'hbRemark': '2级',
          'name': '打算',
          'value': '5f42758d-cfdb-4816-acce-be9d497486ec'
        }, {
          'hbType': '1',
          'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
          'children': [{
            'hbType': '2',
            'hbParentId': '77ce8550-8f8b-411c-b89e-c675241064f2',
            'hbRemark': '3级',
            'name': '今天新增三级',
            'value': 'd8943ab4-4bca-49e0-9ffb-17e93c0ee984'
          }],
          'hbRemark': '2级',
          'name': '今天新增二级',
          'value': '77ce8550-8f8b-411c-b89e-c675241064f2'
        }, {
          'hbType': '1',
          'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
          'hbRemark': '2级',
          'name': '大龙山',
          'value': '81fcb3ef-020a-4b2e-9b8e-b146ad18bb66'
        }, {
          'hbType': '1',
          'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
          'hbRemark': '2级',
          'name': '冉家坝',
          'value': 'b4f86107-9833-4eaa-aa66-da6157b681b9'
        }, {
          'hbType': '1',
          'hbParentId': '49dd73da-b3e2-4a86-9b24-438f9b60f628',
          'children': [{
            'hbType': '2',
            'hbParentId': 'f6b8174b-c691-4650-b60f-d85488ee86cd',
            'hbRemark': '3级',
            'name': 'AAA',
            'value': '15e6baac-6e92-40b0-b955-77083e5ba06e'
          }],
          'hbRemark': '2级',
          'name': 'AA',
          'value': 'f6b8174b-c691-4650-b60f-d85488ee86cd'
        }],
        'hbRemark': '1级',
        'name': '爱车',
        'value': '49dd73da-b3e2-4a86-9b24-438f9b60f628'
      }, {
        'hbType': '0',
        'hbParentId': '0',
        'children': [{
          'hbType': '1',
          'hbParentId': '5471d0fc-4d42-493b-8875-f2ea71613658',
          'children': [{
            'hbType': '2',
            'hbParentId': '49a36108-124e-4aec-b075-e19dd93461ba',
            'hbRemark': '3级',
            'name': '二大爷',
            'value': 'e845a902-cd32-4096-9fc0-c916089f37f6'
          }],
          'hbRemark': '2级',
          'name': '我去',
          'value': '49a36108-124e-4aec-b075-e19dd93461ba'
        }, {
          'hbType': '1',
          'hbParentId': '5471d0fc-4d42-493b-8875-f2ea71613658',
          'children': [{
            'hbType': '2',
            'hbParentId': '8ec1ade7-041e-482d-a689-42a04192c37b',
            'hbRemark': '3级',
            'name': '吃仨',
            'value': 'c0d65f51-e081-4d32-9849-a485e9784fff'
          }],
          'hbRemark': '2级',
          'name': '感受',
          'value': '8ec1ade7-041e-482d-a689-42a04192c37b'
        }],
        'hbRemark': '1级',
        'name': '打野',
        'value': '5471d0fc-4d42-493b-8875-f2ea71613658'
      }, {
        'hbType': '0',
        'hbParentId': '0',
        'children': [{
          'hbType': '1',
          'hbParentId': 'b3c1d40d-c869-4740-b459-077b7ebf2dc3',
          'children': [{
            'hbType': '2',
            'hbParentId': '7e8bb226-853f-48d1-aa37-92fd44ef371c',
            'hbRemark': '3级',
            'name': '锦里',
            'value': 'edf835b0-ef66-41e4-b382-5536bb1ee84e'
          }],
          'hbRemark': '2级',
          'name': '成都',
          'value': '7e8bb226-853f-48d1-aa37-92fd44ef371c'
        }],
        'hbRemark': '1级',
        'name': '四川',
        'value': 'b3c1d40d-c869-4740-b459-077b7ebf2dc3'
      }, {
        'hbType': '0',
        'hbParentId': '0',
        'children': [{
          'hbType': '1',
          'hbParentId': 'e38c6167-89d9-409c-a2c0-8fb2f7e52a90',
          'children': [{
            'hbType': '2',
            'hbParentId': 'da4ad947-ca97-404d-8d99-e049986dc096',
            'hbRemark': '3级',
            'name': '赵信',
            'value': 'e7d502a2-b30a-4a46-b823-b34406e9a616'
          }],
          'hbRemark': '2级',
          'name': '盲僧',
          'value': 'da4ad947-ca97-404d-8d99-e049986dc096'
        }],
        'hbRemark': '1级',
        'name': '蒙多',
        'value': 'e38c6167-89d9-409c-a2c0-8fb2f7e52a90'
      }],
      submitedData: null
    }
  },
  components: {
    Cascader
  },
  methods: {
    handleSubmit: function (data) {
      this.submitedData = data
    },
    handleCancel: function () {
      this.submitedData = '*** 已取消 ***'
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#app .container {
  border: 1px solid #456e8f;
  border-radius: 5px;
  padding: 1em;
  margin: 1em auto;
  /*width: 300px;*/
  min-width: 200px;
  max-width: 300px;
}
.after-submit {
  margin: 1em 0;
  font-size: 12px;
}
#app .instruction textarea {
  /*width: 300px;*/
  min-width: 200px;
  max-width: 300px;
  height: 20em;
}
#app .instruction h4 {
  color: #5780a1;
  font-size: 12px;
}
#app .instruction p {
  font-size: 12px;
}
</style>
