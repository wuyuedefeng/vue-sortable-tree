<template>
  <div class="sortable-tree" :class="{'empty-node': data[attr]}" :draggable="data[attr]" @dragstart.stop="dragStart()" @dragover.prevent @dragenter.stop.prevent="dragEnter()"
       @dragleave.stop="dragLeave()" @drop.stop="drop" @dragend.stop.prevent="dragEnd">
    <div class="content">
      <span>{{data[attr]}}</span>
    </div>
    <ul v-if="hasChildren(data)">
      <li v-for="(item, index) in children" :class="{'parent-li': hasChildren(item), 'exist-li': item[attr], 'blank-li': !item[attr]}">
        <sortable-tree :data="item" :attr="attr" :parentData="data" :idx="index" :dragInfo="dragInfo"></sortable-tree>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'SortableTree',
  props: {
    data: {
      type: Object
    },
    attr: {
      type: String,
      default: 'name'
    },
    parentData: {
      type: Object
    },
    idx: {
      type: Number, // v-for 的索引，用于相邻节点的判别
      default: -1
    },
    dragInfo: {
      type: Object,
      default: () => {
        return {
          data: null, // vm 的数据
          vm: null, // 被拖动组件
          vmIdx: -1,
          parentData: null // vm 的容器数据
        }
      }
    }
  },
  data () {
    return {
      dragObj: this.dragInfo
    }
  },
  computed: {
    children () {
      // 举例：原本是 [N1, N2, N3]
      let { children } = this.data
      if (!children || !children.length) return []

      let _children = []
      children.forEach(child => _children.push({}, child))
      _children.push({})

      // 最后生成 [E1, N1, E2, N2, E3, N3, E4]（其中 N 表示节点，E 表示空节点）
      return _children
    },
    isParent () { // 拖放限制 1：判断“我”是否为被拖动节点的父节点
      return this.data === this.dragObj.parentData
    },
    isNextToMe () { // 拖放限制 2：判断“我”是否为被拖动节点的相邻节点
      return this.parentData === this.dragObj.parentData && Math.abs(this.idx - this.dragObj.vmIdx) === 1
    },
    isMeOrMyAncestor () { // 判断被拖动节点是否为“我”自身或“我”的祖先
      let data = this.data
      while (data) {
        if (data === this.dragObj.data) {
          console.log(true)
          return true
        }
        data = data.$parent
      }
      return false
    },
    isAllowToDrop () { // 上述拖放限制条件的综合
      return !(this.isNextToMe || this.isParent || this.isMeOrMyAncestor)
    }
  },
  methods: {
    hasChildren (item) {
      return item.children && item.children.length
    },
    dragStart () { // 被拖动元素
      this.dragObj.data = this.data
      this.dragObj.vm = this.$el
      this.dragObj.vmIdx = this.idx
      this.dragObj.parentData = this.parentData
    },
    dragEnter () { // 作用在目标元素
      this.dragObj.vm.classList.add('draging')
      if (!this.isAllowToDrop) return
//      console.log(this.$el)
      this.$el.classList.add('droper')
    },
    dragLeave (data) { // 作用在目标元素
      this.$el.classList.remove('droper')
    },
    // 在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。
    drop () { // 目标元素
      this.dragObj.vm.classList.remove('draging')
      this.$el.classList.remove('droper')
      if (!this.isAllowToDrop) return
      // 无论如何都直接删除被拖动节点
      let index = this.dragObj.parentData.children.indexOf(this.dragObj.data)
      this.dragObj.parentData.children.splice(index, 1)
      // 拖入空节点，成其兄弟（使用 splice 插入节点）
      this.parentData.children.splice(this.idx / 2, 0, this.dragObj.data)
    },
    dragEnd () {
    }
  },
  components: {
  },
  updated () {
    this.data.$parent = this.parentData
  },
  mounted () {
    this.data.$parent = this.parentData
  }
}
</script>

<style lang="scss" scoped>
.sortable-tree {
  font-size: 16px;
  overflow: hidden;

  .content {
    min-height: 10px;
  }

  ul, li {
    margin: 0;
    padding: 0;
  }

  ul {
    position: relative;
    display: list-item;
    list-style: none;
    &:empty {
      width: 0;
      height: 0;
    }
  }

  li {
    position: relative;
    padding: 10px 0 0 24px;
  }
}
/* 位置相关 */
.sortable-tree {
  li {
    position: relative;

    &:before, &:after {
      position: absolute;
      content: '';
    }
    &:before {
      width: 24px;
      height: 100%;
      left: 0;
      top: -10px;
      /*background: red;*/
      border-left: 1px solid #999;
    }
    &:after {
      width: 22px;
      height: 20px;
      top: 22px;
      left: 0;
      border-top: 1px solid #999;
    }

    &.parent-li:nth-last-child(2):before {
      width: 24px;
      height: 32px; // 32为1个li的高度
      left: 0;
      top: -10px;
      border-left: 1px solid #999;
    }
    &.blank-li {
      position: absolute;
      z-index: 1;
      margin: 0;
      padding: 0;
      width: 100%;
      height: 10px;
      overflow: hidden;
    }
  }
}
// 拖动相关
.draging {
  background: #EFEEEF;
}
.droper {
  background: lightgreen;
}
</style>
