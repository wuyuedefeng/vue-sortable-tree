# vue-sortable-tree

[demo](http://examples.itrydo.com/vue-sortable-tree/dist/index.html)

 QQ交流群:424580388

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
  <sortable-tree :data="treeData">
    <template scope="{item}">
      <span>{{item.name}}</span>
    </template>
  </sortable-tree>
</template>
```

### params
```
 <sortable-tree :data="treeData" attr="name" childrenAttr="children" mixinParentKey="$parent" @changePosition="changePosition">
    <template scope="{item}">
      <span>{{item.name}}</span>
    </template>
  </sortable-tree>
```

* `data`  must
* `attr`  default: 'name',  if use template can't set
* `childrenAttr`  default: 'children'
* `mixinParentKey` default: '', if wan't get item's parent data can set this, eg: item.$parent
* `changePosition` callback, params options: Object {beforeParent, data, afterParent}
* `closeStateKey`  default: '', key for justify folder open or close
* `dragEnable` default: true, if set false will can't drag


### project demo image
![img](https://github.com/wuyuedefeng/vue-sortable-tree/blob/master/example/src/assets/tree.png)

you can design your style easily with custom template

eg:

![img](https://github.com/wuyuedefeng/vue-sortable-tree/blob/master/example/src/assets/tree-ext.png)

