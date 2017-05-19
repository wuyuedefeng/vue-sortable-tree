<template>
  <div class="sortable-tree-item-drag" :draggable="item[attr]" @dragstart.stop="dragStart($event)" @dragover.prevent @dragenter.stop="dragEnter($event)"
       @dragleave.stop="dragLeave(item, $event)" @drop.stop="drop" @dragend.stop.prevent="dragEnd" :class="{draging: isDraging}">
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
    isDraging () {
      return this.dragObj.data === this.item
    }
  },
  methods: {
    calcStatus () {
      let tItem = this.item
      let equalParent = null
      let equalItemChild = null
      let equalDragChild = null
      let tItemParent = tItem.$parent
      console.log('000', tItemParent.name)
      while (tItemParent) {
        let tDrag = this.dragObj.data
        let tDragParent = tDrag.$parent
        while (tDragParent) {
          if (tItemParent === tDragParent) {
            equalParent = tItemParent
            equalItemChild = tItem
            equalDragChild = tDrag
            break
          } else {
            tDragParent = tDragParent.$parent
          }
        }
        if (equalParent) {
          break
        } else {
          tItemParent = tItemParent.$parent
        }
      }
      console.log(1111, equalParent.name)
      return {
        isToFront: equalParent.children.indexOf(equalItemChild) <= equalParent.children.indexOf(equalDragChild),
        equalParent,
        equalItemChild,
        equalDragChild
      }
    },
    dragStart (event) { // 被拖动元素
      this.dragObj.vm = event.target
      this.dragObj.data = this.item
    },
    dragEnter (event) { // 作用在目标元素
      this.dragObj.vm.classList.add('draging')
      if (this.dragObj.calculating) return
      this.dragObj.calculating = true
      if (this.dragObj.data !== this.item) {
        console.log(this.dragObj.vm, event.target)
        let index = this.dragObj.data.$parent.children.indexOf(this.dragObj.data)
        let status = this.calcStatus()
        this.dragObj.data.$parent.children.splice(index, 1)
        this.dragObj.data.$parent = status.equalParent
        let itemIndex = status.equalParent.children.indexOf(status.equalItemChild)
        if (status.isToFront) {
          status.equalParent.children.splice(itemIndex, 0, status.equalDragChild)
        } else {
          if (itemIndex === status.equalParent.children.length - 1) {
            status.equalParent.children.push(status.equalDragChild)
          } else {
            status.equalParent.children.splice(itemIndex + 1, 0, status.equalDragChild)
          }
        }
      }
      setTimeout(() => {
        this.dragObj.calculating = false
      }, 500)
    },
    dragLeave (data, event) { // 作用在目标元素
    },
    // 在ondragover中一定要执行preventDefault()，否则ondrop事件不会被触发。
    drop (event) { // 目标元素
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
