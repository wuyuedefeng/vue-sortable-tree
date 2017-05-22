<template>
  <div class="hello">
    <sortable-tree :data="treeData">
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
              {name: ''}
            ]
          }
        ]
      }
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
