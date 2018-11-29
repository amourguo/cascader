<template>
  <div class="cascader-wrapper">
    <div class="row">
      <select v-if="newDistricts[0].type === 0" class="field field-lvl1" v-model="newDistricts[0].index">
        <option value="">{{ t(CONSTANTS.PLZ_SELECT_DISTRICT, 1) }}</option>
        <option v-for="(lvl1, idx1) in districts" v-bind:key="lvl1.name" v-bind:value="idx1">{{ lvl1.name }}</option>
      </select>
      <input v-else class="field field-lvl1" v-model="newDistricts[0].text" :placeholder="t(CONSTANTS.PLZ_INPUT_DISTRICT, 1)" />
      <button v-if="newDistricts[0].type === 0" v-on:click="swapType([0])">
        <v-icon name="edit" />
      </button>
      <button v-else v-on:click="swapType([0])">
        <v-icon name="list" />
      </button>
      <button v-on:click="addSubLvl([0])" v-bind:disabled="isEmpty([0])">
        <v-icon name="plus-circle" />
      </button>
    </div>
    <div v-if="errMsgs[0].error[0] > 0" class="error field-lvl1">{{ errMsgs[0].error[1] }}</div>

    <div class='row-lvl-2' v-for="(newLvl2, key2) in (newDistricts[0].children || [])" v-bind:key="[0, key2].join('-')">
      <div class="row">
        <select v-if="newLvl2.type === 0" class="field field-lvl2" v-model="newLvl2.index">
          <option value="">{{ t(CONSTANTS.PLZ_SELECT_DISTRICT, 2) }}</option>
          <option v-for="(lvl2, idx2) in (newDistricts[0].index !== '' ? districts[newDistricts[0].index].children : [])" v-bind:key="lvl2.name" v-bind:value="idx2">{{ lvl2.name }}</option>
        </select>
        <input v-else class="field field-lvl2" v-model="newLvl2.text" :placeholder="t(CONSTANTS.PLZ_INPUT_DISTRICT, 2)" />
        <button v-if="newLvl2.type === 0" v-on:click="swapType([0, key2])">
          <v-icon name="edit" />
        </button>
        <button v-else v-on:click="swapType([0, key2])">
          <v-icon name="list" />
        </button>
        <button v-on:click="addSubLvl([0, key2])" v-bind:disabled="isEmpty([0, key2])">
          <v-icon name="plus-circle" />
        </button>
        <button v-on:click="removeSubLvl([0, key2])">
          <v-icon name="minus-circle" />
        </button>
      </div>
      <div v-if="errMsgs[0].children[key2].error[0] > 0" class="error field-lvl2">{{ errMsgs[0].children[key2].error[1] }}</div>

      <div class='row-lvl-3' v-for="(newLvl3, key3) in (newLvl2.children || [])" v-bind:key="[0, key3].join('-')">
        <div class="row">
          <select v-if="newLvl3.type === 0" class="field field-lvl3" v-model="newLvl3.index">
            <option value="">{{ t(CONSTANTS.PLZ_SELECT_DISTRICT, 3) }}</option>
            <option v-for="(lvl3, idx3) in ((newDistricts[0].index !== '' && newLvl2.index !== '') ? districts[newDistricts[0].index].children[newLvl2.index].children : [])" v-bind:key="lvl3.name" v-bind:value="idx3">{{ lvl3.name }}</option>
          </select>
          <input v-else class="field field-lvl3" v-model="newLvl3.text" :placeholder="t(CONSTANTS.PLZ_INPUT_DISTRICT, 3)" />
          <button v-if="newLvl3.type === 0" v-on:click="swapType([0, key2, key3])">
            <v-icon name="edit" />
          </button>
          <button v-else v-on:click="swapType([0, key2, key3])">
            <v-icon name="list" />
          </button>
          <button v-on:click="removeSubLvl([0, key2, key3])">
            <v-icon name="minus-circle" />
          </button>
        </div>
        <div v-if="errMsgs[0].children[key2].children[key3].error[0] > 0" class="error field-lvl3">{{ errMsgs[0].children[key2].children[key3].error[1] }}</div>
      </div>

    </div>

    <button class="submit" v-on:click="cascaderSubmit" v-bind:disabled="hasError()">
      提交
    </button>

  </div>
</template>

<script>
/* Pick one way between the 2 following ways */
// import all icons if you don't care about bundle size
// import 'vue-awesome/icons'

// or only import the icons you use to reduce bundle size
import 'vue-awesome/icons/edit'
import 'vue-awesome/icons/list'
import 'vue-awesome/icons/plus-circle'
import 'vue-awesome/icons/minus-circle'

import Icon from 'vue-awesome/components/Icon'

export default {
  name: 'Cascader',
  components: {
    'v-icon': Icon
  },
  data () {
    return {
      newDistricts: [
        {
          index: '',
          text: '',
          type: 0, // 0:Dropdown, 1:Text
          // error feature has been moved to errMsgs
          // 0: No Errors, 1: Empty dist name, 2: invalid swapping, 3: Incomplete lvls, 4: Duplicated
          error: [0, 'No Errors'],
          children: null
        }
      ],
      CONSTANTS: {
        PLZ_SELECT_DISTRICT: '请选择％s1级片区',
        PLZ_INPUT_DISTRICT: '请输入％s1级片区',
        INVALID_SWAPPING: '⚠ 只有当％s1级片区为下拉菜单时，％s2级片区才能切换为下拉菜单',
        PLZ_COMPLETE_DISTRICT: '⚠ 请补足三个级别的片区',
        DISTRICT_NAME_REQUIRED: '⚠ 请选择或输入％s1级片区',
        DUPLICATED_DISTRICT: '⚠ 片区重复'
      }
    }
  },
  // Declaring props
  props: {
    districts: {
      type: Array,
      default: function () {
        return []
      }
    }
  },
  methods: {
    cascaderSubmit: function () {
      const normalizedDists = this.normalizeNewDists()
      if (normalizedDists && normalizedDists.length > 0) {
        this.$emit('cascader-submit', normalizedDists)
      }
    },
    normalizeNewDists: function () {
      const retDists = []
      const specimen = this.newDistricts[0].children
      if (!specimen || specimen.length < 1) {
        return []
      }
      const name1 = this.newDistricts[0].type === 0 ? this.getNameByIdx([0]) : this.newDistricts[0].text
      retDists[0] = {
        name: name1,
        children: []
      }

      for (let i = 0; i < specimen.length; i++) {
        const name2 = specimen[i].type === 0 ? this.getNameByIdx([0, specimen[i].index]) : specimen[i].text
        retDists[0].children[i] = {
          name: name2,
          children: []
        }
        const specimen2 = specimen[i].children || []
        for (let j = 0; j < specimen2.length; j++) {
          const name3 = specimen2[j].type === 0 ? this.getNameByIdx([0, specimen[i].index, specimen2[j].index]) : specimen2[j].text
          retDists[0].children[i].children[j] = {
            name: name3,
            children: null
          }
        }
      }
      return retDists
    },
    swapType: function (arrLvl) {
      const lvlDepth = arrLvl.length
      let arrLvlObj = this.newDistricts
      let parentLvlObj = this.newDistricts
      for (let dpth = 0; dpth < lvlDepth; dpth++) {
        const curLvlIdx = arrLvl[dpth]
        if (dpth === lvlDepth - 1) {
          arrLvlObj = arrLvlObj[curLvlIdx]
        } else {
          parentLvlObj = arrLvlObj[curLvlIdx]
          arrLvlObj = arrLvlObj[curLvlIdx].children
        }
      }
      if (arrLvlObj.type === 1 && parentLvlObj.index === '') {
        const formerError = arrLvlObj.error
        arrLvlObj.error = [2, this.t(this.CONSTANTS.INVALID_SWAPPING, lvlDepth - 1, lvlDepth)]
        setTimeout(() => { arrLvlObj.error = formerError }, 2000)
        return
      }
      arrLvlObj.type = arrLvlObj.type === 0 ? 1 : 0
      // setting textbox' intial value base on dropdown value(name)
      if (arrLvlObj.type === 1 && arrLvlObj.index !== '') {
        let context = this.newDistricts
        const arrIdx = arrLvl.map((lvl) => {
          context = (context.children || context)[lvl]
          return context.index
        })
        arrLvlObj.text = this.getNameByIdx(arrIdx)
      }
    },
    addSubLvl: function (arrLvl) {
      const lvlDepth = arrLvl.length
      let arrLvlObj = this.newDistricts
      for (let dpth = 0; dpth < lvlDepth; dpth++) {
        const curLvlIdx = arrLvl[dpth]
        if (dpth === lvlDepth - 1) {
          arrLvlObj = arrLvlObj[curLvlIdx]
        } else {
          arrLvlObj = arrLvlObj[curLvlIdx].children
        }
      }
      const emptyLvlObj = {
        index: '',
        text: '',
        type: arrLvlObj.type, // 0:Dropdown, 1:Text
        error: [0, 'No Errors'],
        children: null
      }
      if (arrLvlObj.children && Array.isArray(arrLvlObj.children)) {
        arrLvlObj.children.push(emptyLvlObj)
      } else {
        arrLvlObj.children = [emptyLvlObj]
      }
    },
    removeSubLvl: function (arrLvl) {
      const lvlDepth = arrLvl.length
      let arrLvlObj = this.newDistricts
      for (let dpth = 0; dpth < lvlDepth; dpth++) {
        const curLvlIdx = arrLvl[dpth]
        if (dpth === lvlDepth - 1) {
          arrLvlObj.splice(curLvlIdx, 1)
        } else {
          arrLvlObj = arrLvlObj[curLvlIdx].children
        }
      }
    },
    isEmpty: function (arrLvl) {
      const lvlDepth = arrLvl.length
      let arrLvlObj = this.newDistricts
      for (let dpth = 0; dpth < lvlDepth; dpth++) {
        const curLvlIdx = arrLvl[dpth]
        if (dpth === lvlDepth - 1) {
          arrLvlObj = arrLvlObj[curLvlIdx]
        } else {
          arrLvlObj = arrLvlObj[curLvlIdx].children
        }
      }

      if (arrLvlObj.type === 0) {
        const isIdxEmpty = arrLvlObj.index === ''
        return isIdxEmpty
      } else {
        const isTxtEmpty = arrLvlObj.text === ''
        return isTxtEmpty
      }
    },
    hasError: function () {
      for (let i = 0; i < this.errMsgs.length; i++) {
        if (this.errMsgs[i].error[0] > 0) {
          return true
        }
        const lvl2Dist = this.errMsgs[i].children || []
        for (let j = 0; j < lvl2Dist.length; j++) {
          if (lvl2Dist[j].error[0] > 0) {
            return true
          }
          const lvl3Dist = lvl2Dist[j].children || []
          for (let k = 0; k < lvl3Dist.length; k++) {
            if (lvl3Dist[k].error[0] > 0) {
              return true
            }
          }
        }
      }
      return false
    },
    checkDuplicate: function (arrName) {
      const newDists = arrName
      for (let i = 0; i < this.districts.length; i++) {
        if (this.districts[i].name.trim() === newDists[0].trim()) {
          const lvl2Dist = this.districts[i].children || []
          for (let j = 0; j < lvl2Dist.length; j++) {
            if (lvl2Dist[j].name.trim() === newDists[1].trim()) {
              const lvl3Dist = lvl2Dist[j].children || []
              for (let k = 0; k < lvl3Dist.length; k++) {
                if (lvl3Dist[k].name.trim() === newDists[2].trim()) {
                  return true
                }
              }
            }
          }
        }
      }
      return false
    },
    checkOuterDuplicate: function () {
      const specimen = this.newDistricts[0].children
      if (!specimen || specimen.length < 1) {
        return []
      }
      const retDupList = []

      for (let i = 0; i < specimen.length; i++) {
        const specimen2 = specimen[i].children || []
        for (let j = 0; j < specimen2.length; j++) {
          const name1 = this.newDistricts[0].type === 0 ? this.getNameByIdx([0]) : this.newDistricts[0].text
          const name2 = specimen[i].type === 0 ? this.getNameByIdx([0, specimen[i].index]) : specimen[i].text
          const name3 = specimen2[j].type === 0 ? this.getNameByIdx([0, specimen[i].index, specimen2[j].index]) : specimen2[j].text
          if (name1 && name2 && name3) {
            if (this.checkDuplicate([name1, name2, name3])) {
              retDupList.push([0, i, j])
            }
          }
        }
      }
      return retDupList
    },
    checkInnerDuplicate: function () {
      const specimen = this.newDistricts[0].children
      if (!specimen) {
        return []
      }

      for (let i = 0; i < specimen.length; i++) {
        if (specimen.length > 1) {
          for (let ii = i + 1; ii < specimen.length; ii++) {
            const name1 = specimen[i].type === 0 ? this.getNameByIdx([0, specimen[i].index]) : specimen[i].text
            const name2 = specimen[ii].type === 0 ? this.getNameByIdx([0, specimen[ii].index]) : specimen[ii].text
            if (name1 === name2 && name1 !== '' && name1 !== undefined) {
              return [
                [0, i],
                [0, ii]
              ]
            }
          }
        }

        const specimen2 = specimen[i].children || []
        if (specimen2.length > 1) {
          for (let j = 0; j < specimen2.length; j++) {
            for (let jj = j + 1; jj < specimen2.length; jj++) {
              const name21 = specimen2[j].type === 0 ? this.getNameByIdx([0, specimen[i].index, specimen2[j].index]) : specimen2[j].text
              const name22 = specimen2[jj].type === 0 ? this.getNameByIdx([0, specimen[i].index, specimen2[jj].index]) : specimen2[jj].text
              if (name21 === name22 && name21 !== '' && name21 !== undefined) {
                return [
                  [0, i, j],
                  [0, i, jj]
                ]
              }
            }
          }
        }
      }
      return []
    },
    getNameByIdx: function (arrIdx) {
      const idxDepth = arrIdx.length
      let objDist = this.districts
      for (let i = 0; i < idxDepth; i++) {
        if (objDist[arrIdx[i]]) {
          if (i === idxDepth - 1) {
            objDist = objDist[arrIdx[i]]
          } else {
            if (objDist[arrIdx[i]].children) {
              objDist = objDist[arrIdx[i]].children
            }
          }
        }
      }
      return objDist.name
    },
    isDuplicatedItem: function (item, list) {
      const listLen = list.length
      const itemLen = item.length
      if (listLen < 1) {
        return false
      }
      if (itemLen !== list[0].length) {
        return false
      }
      for (let i = 0; i < listLen; i++) {
        /*
        let isSame = true
        for (let j = 0; j < itemLen; j++) {
          if(item[j] !== list[i][j]) {
            isSame = false
          }
        }
        if (isSame) {
          return false
        }
        */
        if (JSON.stringify(list[i]) === JSON.stringify(item)) {
          return true
        }
      }
      return false
    },
    t: function (str, s1, s2) {
      let retStr = str
      if (s1) {
        retStr = retStr.replace(/％s1/, s1)
      }
      if (s2) {
        retStr = retStr.replace(/％s2/, s2)
      }
      return retStr
    }
  },
  computed: {
    errMsgs: { // moved to computed to avoid infinite loop
      get: function () {
        const dupItems = [...this.checkInnerDuplicate(), ...this.checkOuterDuplicate()]
        const retErrs = []
        const lvl1Dist = this.newDistricts
        for (let i = 0; i < lvl1Dist.length; i++) {
          let err1 = []
          const isEmpty1 = (
            lvl1Dist[i].type === 0 && lvl1Dist[i].index === ''
          ) || (
            lvl1Dist[i].type === 1 && lvl1Dist[i].text === ''
          )

          if (!isEmpty1 && (!lvl1Dist[i].children || lvl1Dist[i].children.length < 1)) {
            err1 = [3, this.CONSTANTS.PLZ_COMPLETE_DISTRICT]
          } else if (isEmpty1) {
            err1 = [1, this.t(this.CONSTANTS.DISTRICT_NAME_REQUIRED, 1)]
          } else { // if (!isIdxEmpty)
            err1 = [0, 'No Errors']
          }
          retErrs[i] = {
            error: err1,
            children: []
          }

          const lvl2Dist = lvl1Dist[i].children || []
          for (let j = 0; j < lvl2Dist.length; j++) {
            let err2 = []
            const isEmpty2 = (
              lvl2Dist[j].type === 0 && lvl2Dist[j].index === ''
            ) || (
              lvl2Dist[j].type === 1 && lvl2Dist[j].text === ''
            )

            if (lvl2Dist[j].error[0] > 0) {
              err2 = lvl2Dist[j].error
            } else if (!isEmpty2 && (!lvl2Dist[j].children || lvl2Dist[j].children.length < 1)) {
              err2 = [3, this.CONSTANTS.PLZ_COMPLETE_DISTRICT]
            } else if (isEmpty2) {
              err2 = [1, this.t(this.CONSTANTS.DISTRICT_NAME_REQUIRED, 2)]
            } else if (this.isDuplicatedItem([i, j], dupItems)) {
              err2 = [4, this.CONSTANTS.DUPLICATED_DISTRICT]
            } else { // if (!isIdxEmpty)
              err2 = [0, 'No Errors']
            }

            retErrs[i].children[j] = {
              error: err2,
              children: []
            }
            const lvl3Dist = lvl2Dist[j].children || []
            for (let k = 0; k < lvl3Dist.length; k++) {
              let err3 = []

              const isEmpty3 = (
                lvl3Dist[k].type === 0 && lvl3Dist[k].index === ''
              ) || (
                lvl3Dist[k].type === 1 && lvl3Dist[k].text === ''
              )

              if (lvl3Dist[k].error[0] > 0) {
                err3 = lvl3Dist[k].error
              } else if (isEmpty3) {
                err3 = [1, this.t(this.CONSTANTS.DISTRICT_NAME_REQUIRED, 3)]
              } else if (this.isDuplicatedItem([i, j, k], dupItems)) {
                err3 = [4, this.CONSTANTS.DUPLICATED_DISTRICT]
              } else { // if (!isIdxEmpty)
                err3 = [0, 'No Errors']
              }

              retErrs[i].children[j].children[k] = {
                error: err3,
                children: null
              }
            }
          }
        }
        return retErrs
      },
      set: function (val) {
        // not in use
        // this.errorMessage = val
      }
    }
  },
  watch: {
    // not in use
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
  background-color: none;
  border: none;
  padding: 0;
  margin: 0 0 0 7px;
  cursor: pointer;
}
.cascader-wrapper button:disabled {
  color: #a0a0a0;
  background-color: none;
  border: none;
  padding: 0;
  margin: 0 0 0 7px;
  cursor: not-allowed;
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
  margin-top: 1em;
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
  margin-top: 1em;
}
</style>
