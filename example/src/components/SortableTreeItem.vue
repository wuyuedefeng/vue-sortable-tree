<template>
  <div class="sortable-tree-item-drag" :draggable="item[attr]" @dragstart.stop="dragStart(item, $event)" @dragover.prevent @dragenter.stop="dragEnter(item, $event)"
       @dragleave.stop="dragLeave(item, $event)" @drop.stop="drop" @dragend.stop.prevent="dragEnd">
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
    clearBgColor () { // 清理样式
      this.$el.style.backgroundColor = ''
    },
    dragStart (data, event) { // 被拖动元素
      this.dragObj.vm = event.target
      this.dragObj.data = data
    },
    dragEnter (data, event) { // 作用在目标元素
      console.log('dragEnter', data, this.dragObj.vm)
      this.dragObj.vm.classList.add('droper')
    },
    dragLeave (data, event) { // 作用在目标元素
      console.log('dragLeave', data)
    },
    // 在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。
    drop (event) { // 目标元素
      console.log(this.dragObj, event.target, this.item)
    },
    dragEnd () {
    }
  },
  components: {
  }
}
</script>

<style>
  .droper {
    background: #EFEEEF;
  }
</style>
