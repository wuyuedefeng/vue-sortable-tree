<template>
  <div class="sortable-tree-item-drag" :draggable="item[attr]" @dragstart.stop="dragStart($event)" @dragover.prevent @dragenter.stop="dragEnter(item, $event)"
       @dragleave.stop="dragLeave(item, $event)" @drop.stop="drop" @dragend.stop.prevent="dragEnd">
    <slot :item="item"></slot>
  </div>
</template>

<script>
export default {
  name: 'SortableTreeItem',
  props: ['item', 'parentItem', 'attr', 'dragInfo'],
  data () {
    return {
      dragObj: this.dragInfo
    }
  },
  methods: {
    dragStart (event) { // 被拖动元素
      this.dragObj.vm = event.target
      this.dragObj.data = this.item
    },
    dragEnter (data, event) { // 作用在目标元素
      console.log('dragEnter')
      this.dragObj.vm.classList.add('draging')
    },
    dragLeave (data, event) { // 作用在目标元素
      console.log('dragLeave', data)
    },
    // 在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。
    drop (event) { // 目标元素
      console.log(this.dragObj, event.target, this.item)
      this.dragObj.vm.classList.remove('draging')
    },
    dragEnd () {
    }
  },
  mounted () {
    this.item.$parent = this.parentItem
  },
  components: {
  }
}
</script>

<style>
  .draging {
    background: #EFEEEF;
  }
</style>
