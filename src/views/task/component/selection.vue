<template>
    <div class="selection-component">
      <div class="selection-show" @click="toggleDrop">
        <!-- <span>{{ selections[nowIndex].name }}</span> -->
        <span>{{ tip }}</span>
        <div class="arrow"></div>
      </div>
      <div class="selection-list" v-if="isDrop">
        <ul class="select-ul">
          <li class="ivu-select-item" v-for="(item, index) in selections" @click="chooseSelection(index)">{{ item.name }}</li>
        </ul>
      </div>
    </div>
</template>

<script>
export default {
  props: {
    selections: {
      type: Array,
      default: [{
        name: '',
        id: 0
      }]
    }
  },
  data () {
    return {
      isDrop: false,
      nowIndex: 0,
      tip: '请选择物料' 
    }
  },
  methods: {
    toggleDrop () {
      this.isDrop = !this.isDrop
    },
    chooseSelection (index) {
      this.tip = this.selections[index].name
      this.nowIndex = index
      this.isDrop = false
      console.log("this.nowIndex;"+ this.nowIndex)
      console.log("this.selections[this.nowIndex].name:"+this.selections[this.nowIndex].name)
      this.$emit('on-change', this.selections[this.nowIndex])
      //this.$emit('on-change', 'name')
    }
  }
}
</script>
<style scoped>
.selection-component {
  position: relative;
  display: inline-block;
}
.selection-show {
  border: 1px solid #e3e3e3;
  padding: 0 20px 0 10px;
  display: inline-block;
  position: relative;
  cursor: pointer;
  line-height: 28px;
  border-radius: 3px;
  background: #fff;
  width: 100px;
}
.selection-show .arrow {
  display: inline-block;
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 5px solid #e3e3e3;
  width: 0;
  height: 0;
  margin-top: -1px;
  margin-left: 6px;
  margin-right: -14px;
  vertical-align: middle;
}
.selection-list {
  display: inline-block;
  position: absolute;
  left: 0;
  top: 25px;
  width: 100%;
  background: #fff;
  border-top: 1px solid #e3e3e3;
  border-bottom: 1px solid #e3e3e3;
  z-index: 5;
}
.selection-list li {
  padding: 5px 15px 5px 10px;
  border-left: 1px solid #e3e3e3;
  border-right: 1px solid #e3e3e3;
  cursor: pointer;
  background: #fff;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;

}
.selection-list li:hover {
  background: #e3e3e3;
}
</style>
