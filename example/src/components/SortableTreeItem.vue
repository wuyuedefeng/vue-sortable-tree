<template>
  <div class="sortable-tree-item-drag" :draggable="item[attr]" @dragstart.stop="dragStart()" @dragover.prevent @dragenter.stop="dragEnter()"
       @dragleave.stop="dragLeave(item)" @drop.stop="drop" @dragend.stop.prevent="dragEnd">
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
  computed: {
  },
  methods: {
    dragStart () { // 被拖动元素
      this.dragObj.vm = this.$el
      this.dragObj.data = this.item
    },
    dragEnter () { // 作用在目标元素
      this.dragObj.vm.classList.add('draging')
      if (this.dragObj.data !== this.item) {
        let status = this.calcStatus()
        console.log(this.dragObj.data, this.item, status.isToFront, status)
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
  .droper {
    background: yellow;
  }
</style>
