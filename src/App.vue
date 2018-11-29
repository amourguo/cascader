<template>
  <div id="app">
    <!-- router-view/ -->
    <div class="container">
      <h2>新增片区</h2>
      <Cascader
        :districts="districts"
        @cascader-submit="handleSubmit"
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
    name: '北京',
    children: [
      {
        name: '海淀',
        children: [
          {
            name: '黄庄',
            children: null
          },
          {
            name: '五道口',
            children: null
          }
        ]
      },
      {
        name: '通州',
        children: [
          {
            name: '永顺',
            children: null
          },
          {
            name: '马驹桥',
            children: null
          }
        ]
      }
    ]
  },
  {
    name: '河北',
    children: [
      {
        name: '石家庄',
        children: [
          {
            name: '长安区',
            children: null
          },
          {
            name: '桥西区',
            children: null
          }
        ]
      },
      {
        name: '唐山',
        children: [
          {
            name: '路北',
            children: null
          },
          {
            name: '路南',
            children: null
          }
        ]
      }
    ]
  }
]
      </textarea>
      <h4>输出：emit事件：'cascader-submit'</h4>
      <p>事件携带参数 {Array} [ {Object}, ... ]</p>
      <p>&lt;Cascader :cascader-submit="{Function} eventHandler" /&gt;</p>
      <textarea>
[
  {
    name: '北京',
    children: [
      {
        name: '海淀',
        children: [
          {
            name: '二里庄',
            children: null
          },
          {
            name: '成府路',
            children: null
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
      districts: [
        {
          name: '北京',
          children: [
            {
              name: '海淀',
              children: [
                {
                  name: '黄庄',
                  children: null
                },
                {
                  name: '五道口',
                  children: null
                }
              ]
            },
            {
              name: '通州',
              children: [
                {
                  name: '永顺',
                  children: null
                },
                {
                  name: '马驹桥',
                  children: null
                }
              ]
            }
          ]
        },
        {
          name: '河北',
          children: [
            {
              name: '石家庄',
              children: [
                {
                  name: '长安区',
                  children: null
                },
                {
                  name: '桥西区',
                  children: null
                }
              ]
            },
            {
              name: '唐山',
              children: [
                {
                  name: '路北',
                  children: null
                },
                {
                  name: '路南',
                  children: null
                }
              ]
            }
          ]
        }
      ],
      submitedData: null
    }
  },
  components: {
    Cascader
  },
  methods: {
    handleSubmit: function (data) {
      this.submitedData = data
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
