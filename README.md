# vue-sortable-tree

[demo](http://examples.itrydo.com/vue-sortable-tree/dist/index.html)

### install
```
npm install vue-sortable-tree --save
```
global register
```
import SortableTree from 'vue-sortable-tree'
Vue.component(SortableTree.name, SortableTree)
```

partial register
```
import SortableTree from 'vue-sortable-tree'
# then in component
components: {
 [SortableTree.name]: SortableTree
}
```
### usage
```
<template>
  <sortable-tree :data="treeData" attr="name"></sortable-tree>
</template>

<script>
export default {
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
              { name: '1-3' }
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
  }
}
</script>
export def
```




### project demo image
![img](https://github.com/wuyuedefeng/vue-sortable-tree/blob/master/example/src/assets/tree.png)

