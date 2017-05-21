<template>
  <div class="hello">
    <sortable-tree :data="treeData"></sortable-tree>
  </div>
</template>

<script>
import SortableTree from './SortableTree.vue'
export default {
  name: 'hello',
  template: '#sortable-child-tpl',
  data () {
    return {
      treeData: {
        name: 'root',
        children: [
          { name: '1',
            children: [
              { name: '1-1' },
              { name: '1-2',
                children: [
                  { name: '1-2-1' }
                ]
              },
              { name: '1-3' },
              { name: '1-4',
                children: [
                  { name: '1-4-1',
                    children: [
                      { name: '1-4-1-1',
                        children: [
                          { name: '1-4-1-1-1' }
                        ]
                      }
                    ]
                  },
                  { name: '1-4-2' }
                ]
              }
            ]
          },
          {
            name: '2',
            children: [
              { name: '2-1' }
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
