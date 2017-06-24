<template>
  <div class="hello">
    <button @click="consoleData">consoleData</button>
    <sortable-tree :data="treeData" mixinParentKey="$parent" @changePosition="changePosition" :dragEnable="true" closeStateKey="$foldClose">
      <template scope="{item}">
        <div class="custom-tree-content" :class="{'exitChild': item.children && item.children.length}">
          <span v-if="item['$foldClose'] && item.children && item.children.length" @click="changeState(item)">▶</span>
          <span v-else-if="!item['$foldClose'] && item.children && item.children.length" @click="changeState(item)">▼</span>
          <span>{{item.name}}</span>
        </div>
      </template>
    </sortable-tree>
  </div>
</template>

<script>
// import SortableTree from './SortableTree.vue'
import SortableTree from 'vue-sortable-tree'
export default {
  name: 'hello',
  data () {
    return {
      treeData: {
        name: 'root',
        children: [
          {
            name: '2',
            children: [
              { name: '2-1' },
              {
                name: '2-2',
                children: [{
                  name: '3-2-1',
                  children: [
                    { name: '3-2-1-1' },
                    { name: '3-2-1-2' }
                  ]
                }]
              }
            ]
          },
          {
            name: '3',
            children: [
              { name: '3-1' },
              { name: '3-2' }
            ]
          }
        ]
      }
    }
  },
  methods: {
    consoleData () {
      console.log(this.treeData)
    },
    changeState (item) {
      this.$set(item, '$foldClose', !item['$foldClose'])
    },
    changePosition (option) {
      console.log('changePosition: ', option)
    }
  },
  components: {
    [SortableTree.name]: SortableTree
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  .custom-tree-content {
    position: relative;
    top: 2px;
    z-index: 1;
    &.exitChild {
      margin-left: -7px;
    }
  }
</style>
