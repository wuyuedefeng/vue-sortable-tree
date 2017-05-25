<template>
  <div class="hello">
    <button @click="consoleData">consoleData</button>
    <sortable-tree :data="treeData" mixinParentKey="$parent" @changePosition="changePosition" :draggable="true">
      <template scope="{item}">
        <span>{{item.name}}</span>
      </template>
    </sortable-tree>
  </div>
</template>

<script>
import SortableTree from './SortableTree.vue'
// import SortableTree from 'vue-sortable-tree'
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
</style>
