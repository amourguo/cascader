<template>
  <div class="cascader-wrapper">
    <div class="row">
      <select v-if="lvlType[0] === 0" class="field field-lvl1" v-model="selectedIndex1">
        <option value="">请选择1级片区</option>
        <option v-for="(lvl1, idx1) in districts" v-bind:key="lvl1.name" v-bind:value="idx1">{{ lvl1.name }}</option>
      </select>
      <input v-else class="field field-lvl1" v-model="lvl1Text" placeholder="请输入1级片区" />
      <button v-if="lvlType[0] === 0" v-on:click="swapLvlTyp(0)">
        <v-icon name="edit" />
      </button>
      <button v-else v-on:click="swapLvlTyp(0)">
        <v-icon name="list" />
      </button>
      <button>
        <v-icon name="plus-circle" />
      </button>
    </div>
    <div v-if="errMsg[0] === 0" class="error field-lvl1">{{ errMsg[1] }}</div>
    <div class="row" v-if="isShowDropdown(1)">
      <select v-if="lvlType[1] === 0" class="field field-lvl2" v-model="selectedIndex2">
        <option value="">请选择2级片区</option>
        <option v-for="(lvl2, idx2) in districts[selectedIndex1].children" v-bind:key="lvl2.name" v-bind:value="idx2">{{ lvl2.name }}</option>
      </select>
      <input v-else class="field field-lvl2" v-model="lvl2Text" placeholder="请输入2级片区" />
      <button v-if="lvlType[1] === 0" v-on:click="swapLvlTyp(1)">
        <v-icon name="edit" />
      </button>
      <button v-else v-on:click="swapLvlTyp(1)">
        <v-icon name="list" />
      </button>
      <button>
        <v-icon name="plus-circle" />
      </button>
      <button>
        <v-icon name="minus-circle" />
      </button>
    </div>
    <div v-if="errMsg[0] === 1" class="error field-lvl2">{{ errMsg[1] }}</div>
    <div class="row" v-if="isShowDropdown(2)">
      <select v-if="lvlType[2] === 0" class="field field-lvl3" v-model="selectedIndex3">
        <option value="">请选择3级片区</option>
        <option v-for="(lvl3, idx3) in districts[selectedIndex1].children[selectedIndex2].children" v-bind:key="lvl3.name" v-bind:value="idx3">{{ lvl3.name }}</option>
      </select>
      <input v-else class="field field-lvl3" v-model="lvl3Text" placeholder="请输入3级片区" />
      <button v-if="lvlType[2] === 0" v-on:click="swapLvlTyp(2)">
        <v-icon name="edit" />
      </button>
      <button v-else v-on:click="swapLvlTyp(2)">
        <v-icon name="list" />
      </button>
      <button>
        <v-icon name="plus-circle" />
      </button>
    </div>
    <div v-if="errMsg[0] === 2" class="error field-lvl3">{{ errMsg[1] }}</div>

    <!-- logging starts -->
    <h4 style="margin: 1em 0 0 0;">Index: {{ selectedIndex1 }} - {{ selectedIndex2 }} - {{ selectedIndex3 }}</h4>
    <h4 style="margin: 0 0 1em 0;">{{ getNewDistricts().join(' - ') }}</h4>
    <!-- logging ends -->

    <button class="submit" v-on:click="cascaderSubmit" v-bind:disabled="errMsg[0] > -1">
      提交
    </button>
  </div>
</template>

<script>
// 在不关心打包体积时一次引入全部图标
// import 'vue-awesome/icons'

// 仅引入用到的图标以减小打包体积
import 'vue-awesome/icons/edit'
import 'vue-awesome/icons/list'
import 'vue-awesome/icons/plus-circle'
import 'vue-awesome/icons/minus-circle'
/* 任选一种注册组件的方式 */
import Icon from 'vue-awesome/components/Icon'
export default {
  name: 'Cascader',
  components: {
    'v-icon': Icon
  },
  data () {
    return {
      selectedIndex1: '',
      selectedIndex2: '',
      selectedIndex3: '',
      lvl1Text: '',
      lvl2Text: '',
      lvl3Text: '',
      errorMessage: [-1, 'No messages'], // -1: no errors, 0-2: level 1-3
      lvlType: [0, 0, 0] // 0:Dropdown, 1:Text
    }
  },
  // 声明 props
  props: {
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
      }
    }
  },
  methods: {
    cascaderSubmit: function () {
      if (this.errMsg[0] > -1) {
        return
      }
      this.$emit('cascader-submit', this.getNewDistricts())
    },
    swapLvlTyp: function (lvl) {
      if (
        this.lvlType[lvl] === 1 &&
        this['selectedIndex' + lvl] === ''
      ) {
        this.errMsg = [lvl, '⚠ 只有当' + lvl + '级片区为下拉菜单时，' + (lvl + 1) + '级片区才能切换为下拉菜单']
        setTimeout(() => { this.errMsg = [-1, 'No messages'] }, 2000)
      }
      this.lvlType[lvl] = this.lvlType[lvl] === 0 ? 1 : 0
      this.lvlType = this.lvlType.slice() // just for rerendering
      this['lvl' + (lvl + 1) + 'Text'] = this.getLvlTextByIndex(lvl)
    },
    getLvlTextByIndex: function (lvl) {
      switch (lvl) {
        case 0:
          // 1级片区
          return (
            this.selectedIndex1 !== null &&
            this.selectedIndex1 !== undefined &&
            this.selectedIndex1 !== '' &&
            this.districts[this.selectedIndex1] &&
            this.districts[this.selectedIndex1].name
          ) || ''
          // break
        case 1:
          // 2级片区
          return (
            // this.selectedIndex1 &&
            this.selectedIndex2 !== null &&
            this.selectedIndex2 !== undefined &&
            this.selectedIndex2 !== '' &&
            this.districts[this.selectedIndex1] &&
            this.districts[this.selectedIndex1].children[this.selectedIndex2] &&
            this.districts[this.selectedIndex1].children[this.selectedIndex2].name
          ) || ''
          // break
        default:
          // 3级片区
          return (
            // this.selectedIndex1 &&
            // this.selectedIndex2 &&
            this.selectedIndex3 !== null &&
            this.selectedIndex3 !== undefined &&
            this.selectedIndex3 !== '' &&
            this.districts[this.selectedIndex1] &&
            this.districts[this.selectedIndex1].children[this.selectedIndex2] &&
            this.districts[this.selectedIndex1].children[this.selectedIndex2].children[this.selectedIndex3] &&
            this.districts[this.selectedIndex1].children[this.selectedIndex2].children[this.selectedIndex3].name
          ) || ''
      }
    },
    getNewDistricts: function () {
      const newDists = this.lvlType.map((type, lvl) => {
        if (type === 0) {
          return this.getLvlTextByIndex(lvl)
        }
        return this['lvl' + (lvl + 1) + 'Text']
      })
      return newDists
    },
    isShowDropdown: function (lvl) {
      const parentIndex = this['selectedIndex' + lvl]
      const parentLvlType = this.lvlType[lvl - 1]
      // let isShow = true
      if (
        // this.lvlType[lvl] = 0
        parentIndex !== null &&
        parentIndex !== undefined &&
        parentIndex !== ''
      ) {
        return true
      }
      if (
        parentLvlType !== 0 && this['lvl' + lvl + 'Text'].trim()
      ) {
        this.lvlType[lvl] = 1
        return true
      }
      return false
    },
    checkDuplicate: function () {
      const newDists = this.getNewDistricts()
      for (let i = 0; i < this.districts.length; i++) {
        if (this.districts[i].name === newDists[0]) {
          const lvl2Dist = this.districts[i].children || []
          for (let j = 0; j < lvl2Dist.length; j++) {
            if (lvl2Dist[j].name === newDists[1]) {
              const lvl3Dist = lvl2Dist[j].children || []
              for (let k = 0; k < lvl3Dist.length; k++) {
                if (lvl3Dist[k].name === newDists[2]) {
                  return [2, '⚠ 片区“' + newDists.join('-') + '”已存在，请修改后再提交']
                }
              }
            }
          }
        }
      }
      return [-1, 'No duplicates']
    }
  },
  computed: {
    // newDistricts: function () {
    //   const newDists = this.lvlType.map((dist, lvl) => {
    //     if (dist === 0) {
    //       return this.getLvlTextByIndex(lvl)
    //     }
    //     return this['lvl' + (lvl + 1) + 'Text']
    //   })
    //   return newDists
    // }
    errMsg: {
      get: function () {
        if (this.errorMessage[0] > -1) {
          return this.errorMessage
        }
        const newDists = this.getNewDistricts()
        for (let lvl = 0; lvl < newDists.length; lvl++) {
          if (!newDists[lvl]) {
            return [lvl, '⚠ 请选择或输入' + (lvl + 1) + '级片区']
          }
        }
        const duplicates = this.checkDuplicate()
        if (duplicates[0] > -1) {
          return duplicates
        }
        return [-1, 'No errors']
      },
      set: function (val) {
        this.errorMessage = val
      }
    }
  },
  watch: {
    selectedIndex1: function (val) {
      this.selectedIndex2 = ''
      this.selectedIndex3 = ''
    },
    selectedIndex2: function (val) {
      this.selectedIndex3 = ''
    }
  }
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
  font-size: 12px;
}
.cascader-wrapper .row {
  display: flex;
  flex-flow: row nowrap;
  margin-top: 0.5em;
}
.cascader-wrapper .field {
  flex: 1 auto;
}
.cascader-wrapper .field-lvl1 {
  margin-left: 0;
}
.cascader-wrapper .field-lvl2 {
  margin-left: 2em;
}
.cascader-wrapper .field-lvl3 {
  margin-left: 4em;
}
.cascader-wrapper .error {
  text-align: left;
  color: #f00;
  font-size: 1em;
}
.cascader-wrapper button {
  color: #456e8f;
  border: none;
  padding: 0;
  margin: 0 4px;
  cursor: pointer;
}
.cascader-wrapper input, .cascader-wrapper select {
  border: 1px solid #456e8f;
  border-radius: 5px;
  height: 2em;
  min-height: 2em;
  line-height: 2em;
  font-size: 1em;
  padding: 0 7px;
  white-space: pre;
  box-sizing: border-box;
}
.cascader-wrapper select>option {
  height: 2em;
  line-height: 2em;
  font-size: 1em;
  padding: 0 5px;
  /*white-space: pre;*/
  min-height: 2em;
}
.cascader-wrapper button.submit {
  background-color: #456e8f;
  color: #fff;
  border: 1px solid #a0a0a0;
  padding: 0 2em;
  height: 2em;
  line-height: 2em;
  border-radius: 5px;
  font-size: 14px;
  font-weight: bold;
}
.cascader-wrapper button.submit:disabled {
  background-color: #a0a0a0;
  border: 1px solid #a0a0a0;
  cursor: not-allowed;
  padding: 0 2em;
  height: 2em;
  line-height: 2em;
  border-radius: 5px;
  font-size: 14px;
  font-weight: normal;
}
</style>
