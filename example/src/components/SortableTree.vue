<template>
  <div class="sortable-tree">
    <span>o {{data[attr]}} - {{isParent}} - {{isNextToMe}}</span>
    <ul v-if="hasChildren(data)">
      <li v-for="(item, index) in children" :class="{'parent-li': hasChildren(item)}">
        <div class="sortable-tree-item-drag" :draggable="item[attr]" @dragstart.stop="dragStart(item, index, $event)" @dragover.prevent @dragenter.stop="dragEnter()"
             @dragleave.stop="dragLeave(item)" @drop.stop="drop" @dragend.stop.prevent="dragEnd">
          <sortable-tree :data="item" :attr="attr" :parentData="data" :idx="index" :dragInfo="dragInfo"></sortable-tree>
        </div>
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
    }
  },
  methods: {
    hasChildren (item) {
      return item.children && item.children.length
    },
    dragStart (item, index, event) { // 被拖动元素
      this.dragObj.data = item
      this.dragObj.vm = event.target
      this.dragObj.vmIdx = index
      this.dragObj.parentData = this.data
    },
    dragEnter () { // 作用在目标元素
      this.dragObj.vm.classList.add('draging')
      if (this.dragObj.data !== this.item) {
      }
    },
    dragLeave (data) { // 作用在目标元素
    },
    // 在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。
    drop () { // 目标元素
      this.dragObj.vm.classList.remove('draging')
    },
    dragEnd () {
    }
  },
  components: {
  }
}
</script>

<style lang="scss" scoped>
.sortable-tree {
  font-size: 16px;

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
    &.parent-li:last-child:before {
      width: 24px;
      height: 32px; // 32为1个li的高度
      left: 0;
      top: -10px;
      border-left: 1px solid #999;
    }
  }
}
// 拖动相关
.draging {
  background: #EFEEEF;
}
.droper {
  background: yellow;
}
</style>
