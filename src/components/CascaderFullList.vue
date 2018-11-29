<template>
  <div class="cascader-wrapper">
    <div v-for="(lvl1, idx1) in stateDistricts" v-bind:key="idx1">
      <select class="field-lvl1" v-bind:class="{ 'has-error': showErrMsg([idx1]) }">
        <option v-for="(optLvl1, optIdx1) in stateDistricts" v-bind:key="optIdx1" v-bind:value="optIdx1" v-bind:selected="idx1 === optIdx1">{{ optLvl1.name }}</option>
      </select>
      <span>[+]</span>
      <span class="error-message">{{showErrMsg([idx1])}}</span>
      <div v-for="(lvl2, idx2) in stateDistricts[idx1].children" v-bind:key="idx1 + '.' + idx2">
        <select class="field-lvl2" v-bind:class="{ 'has-error': showErrMsg([idx1, idx2]) }">
          <option v-for="(optLvl2, optIdx2) in stateDistricts[idx1].children" v-bind:key="optIdx2" v-bind:value="optIdx2" v-bind:selected="idx2 === optIdx2">{{ optLvl2.name }}</option>
        </select>
        <span>[+]</span>
        <span class="error-message">{{showErrMsg([idx1, idx2])}}</span>
        <div v-for="(lvl3, idx3) in stateDistricts[idx1].children[idx2].children" v-bind:key="idx1 + '.' + idx2 + '.' + idx3">
          <select class="field-lvl3" v-bind:class="{ 'has-error': showErrMsg([idx1, idx2, idx3]) }">
            <option v-for="(optLvl3, optIdx3) in stateDistricts[idx1].children[idx2].children" v-bind:key="optIdx3" v-bind:value="optIdx3" v-bind:selected="idx3 === optIdx3">{{ optLvl3.name }}</option>
          </select>
          <span class="error-message">{{showErrMsg([idx1, idx2, idx3])}}</span>
        </div>
      </div>
    </div>
    <h3>{{ selectedIndex1 }} - {{ selectedIndex2 }} - {{ selectedIndex3 }}</h3>
    <!-- <h3>{{ normalizedIndex1 }} - {{ normalizedIndex2 }} - {{ normalizedIndex3 }}</h3> -->
    <!-- <input v-model="districts"> -->
    <!-- <button v-on:click="reverseMessage">Reverse</button> -->
    <button v-on:click="cascaderSubmit">
      提交
    </button>
  </div>
</template>

<script>
export default {
  name: 'Cascader',
  data () {
    return {
      selectedIndex1: null,
      selectedIndex2: null,
      selectedIndex3: null,
      errorMsgs: []
    }
  },
  // 声明 props
  // props: ['msg'],
  props: {
    /*
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组／对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
    */
    // selectedIndex1: {
    //   type: Number
    //   // required: true
    //   // default: null
    // },
    // selectedIndex2: {
    //   type: Number
    //   // required: true
    //   // default: null
    // },
    // selectedIndex3: {
    //   type: Number
    //   // required: true
    //   // default: null
    // },
    districts: {
      type: Array,
      default: function () {
        return [
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
                  },
                  {
                    name: ''
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
              },
              {
                name: '朝阳',
                children: null
              },
              {
                name: '顺义',
                children: []
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
          },
          {
            name: '河南',
            children: null
          },
          {
            name: '湖北'
          },
          {
            name: ''
          }
        ]
      }
    }
  },
  methods: {
    // resetLvl2: function() {
    //   this.selectedIndex2 = null
    //   this.selectedIndex3 = null
    //   console.log('>>> resetLvl2 >>>', this)
    // }
    cascaderSubmit: function () {
      const isIntegrated = this.checkIntegrity()
      if (isIntegrated) {
        console.log('>>> pre-cascaderSubmit >>>', this.$emit)
        this.$emit('cascader-submit')
        console.log('>>> post-cascaderSubmit >>>')
      }
    },
    checkIntegrity: function () {
      const incompleteList = []
      this.stateDistricts.forEach(function (itemLvl1, i1) {
        console.log('>>> stateDistricts item lvl1 >>>', itemLvl1.name, itemLvl1.children, itemLvl1, i1)
        if (
          itemLvl1.name.trim() === '' ||
          !itemLvl1.children ||
          itemLvl1.children.length < 1
        ) {
          console.log('>>> checkIntegrity >>> sth is wrong.....')
          incompleteList.push([i1])
        } else {
          itemLvl1.children.forEach(function (itemLvl2, i2) {
            console.log('>>> stateDistricts item lvl2 >>>', itemLvl2, i2)
            if (
              itemLvl2.name.trim() === '' ||
              !itemLvl2.children ||
              itemLvl2.children.length < 1
            ) {
              console.log('>>> checkIntegrity >>> sth is wrong.....')
              incompleteList.push([i1, i2])
            } else {
              itemLvl2.children.forEach(function (itemLvl3, i3) {
                console.log('>>> stateDistricts item lvl3 >>>', itemLvl3, i3)
                if (
                  itemLvl3.name.trim() === ''
                ) {
                  console.log('>>> checkIntegrity >>> sth is wrong.....')
                  incompleteList.push([i1, i2, i3])
                }
              })
            }
          })
        }
      })
      console.log('>>> checkIntegrity >>>', incompleteList)
      const isIntegrated = incompleteList.length < 1
      if (!isIntegrated) {
        this.addErrors(incompleteList, '各级片区均不可缺失')
      }
      return isIntegrated
    },
    addErrors: function (arrIdx, strMsg) {
      console.log('>>> add errors >>> before >>', arrIdx, strMsg, this.errorMsgs)
      this.errorMsgs = []
      arrIdx.forEach((idx) => {
        this.errorMsgs.push([idx, strMsg])
      })
      console.log('>>> add errors >>> after ------- >>', arrIdx, strMsg, this.errorMsgs)
    },
    showErrMsg: function (arrIdx) {
      // const errLen = this.errorMsgs.length
      console.log('>>> showErrMsg ===== ===== this.errorMsgs >>>', this.errorMsgs)
      for (let i in this.errorMsgs) {
        console.log('>>>> showErrMsg >>>>>>>', i, '>>>>', this.errorMsgs[i][0], arrIdx)
        if (this.errorMsgs[i][0].length === arrIdx.length) {
          let allEquals = true
          arrIdx.forEach((idx, lvl) => {
            console.log('>>> idx === idx >>>', idx, lvl, idx === this.errorMsgs[i][0][lvl], idx, this.errorMsgs[i][0][lvl])
            if (idx !== this.errorMsgs[i][0][lvl]) {
              allEquals = false
            }
          })
          if (allEquals) {
            return this.errorMsgs[i][1]
          }
          // return allEquals ? this.errorMsgs[i][1] : null
        }
      }
    }
  },
  computed: {
    // selectedIndex1: {
    //   get: () => {
    //     const retVal = (this.selectedIndex1 !== null && this.selectedIndex1 !== undefined) ? this.selectedIndex1 : null
    //     console.log('>>> getter >>>', this.selectedIndex1, '>>> ret >>>', retVal)
    //     return retVal
    //   },
    //   // only setter is needed
    //   set: (newValue) => {
    //     console.log('>>> setter >>>', newValue, this.selectedIndex1)
    //     if (newValue !== this.selectedIndex1) {
    //       this.selectedIndex2 = null
    //       this.selectedIndex3 = null
    //     }
    //     this.selectedIndex1 = newValue
    //     console.log('>>> selectedIndex >>>', this.selectedIndex1, this.selectedIndex2, this.selectedIndex3)
    //   }
    // }
    stateDistricts: function () {
      return this.districts
    }
  },
  watch: {
    selectedIndex1: function (val) {
      console.log('>>> selectedIndex - 1 >>>', val, this.selectedIndex1, this.selectedIndex2, this.selectedIndex3, this)
      this.selectedIndex2 = null
      this.selectedIndex3 = null
    },
    selectedIndex2: function (val) {
      console.log('>>> selectedIndex - 2 >>>', val, this.selectedIndex1, this.selectedIndex2, this.selectedIndex3, this)
      // this.selectedIndex2 = null
      this.selectedIndex3 = null
    }
  }
  // computed: {
  //   normalizedIndex1: () => {
  //     const idx = this.selectedIndex1
  //     const retVal = (idx !== null && idx !== undefined) ? idx : null
  //     console.log('>>> getter >>>', this.selectedIndex1, '>>> ret >>>', retVal)
  //     return retVal
  //   },
  //   normalizedIndex2: () => {
  //     const retVal = (this.selectedIndex2 !== null && this.selectedIndex2 !== undefined) ? this.selectedIndex2 : null
  //     console.log('>>> getter >>>', this.selectedIndex2, '>>> ret >>>', retVal)
  //     return retVal
  //   },
  //   normalizedIndex3: () => {
  //     const retVal = (this.selectedIndex3 !== null && this.selectedIndex3 !== undefined) ? this.selectedIndex3 : null
  //     console.log('>>> getter >>>', this.selectedIndex3, '>>> ret >>>', retVal)
  //     return retVal
  //   }
  // }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.cascader-wrapper {
  display: flex;
  flex-flow: column nowrap;
}
.cascader-wrapper select {
  width: 10em;
}
.cascader-wrapper .error-message {
  font-size: 12px;
  color: #f00;
}
.cascader-wrapper .has-error {
  border: 1px solid #f00;
}
.field-lvl1 {
  margin-left: 0;
}
.field-lvl2 {
  margin-left: 4em;
}
.field-lvl3 {
  margin-left: 8em;
}
</style>
