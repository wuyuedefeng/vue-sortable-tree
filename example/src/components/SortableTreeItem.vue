<template>
  <div class="sortable-tree-item-drag" :draggable="item[attr]" @dragstart.stop="dragStart(item, $event)" @dragover.prevent @drop.stop="drop" @dragend.stop.prevent="dragEnd">
    <slot :item="item"></slot>
  </div>
</template>

<script>
export default {
  name: 'SortableTreeItem',
  props: ['item', 'attr', 'dragInfo'],
  data () {
    return {
      dragObj: this.dragInfo
    }
  },
  methods: {
    dragStart (data, event) { // 被拖动元素
      this.dragObj.vm = event.target
      this.dragObj.data = data
      event.target.style.background = 'silver'
    },
    // 在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。
    drop (event) { // 目标元素
//      console.log(this.dragObj, event.target, this.item)
    },
    dragEnd () {
    }
  },
  components: {
  }
}
</script>

<style>
</style>
