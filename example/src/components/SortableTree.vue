<template>
  <div>
    <div v-for="item in data">
      <sortable-tree-item :item="item" :attr="attr" :dragInfo="dragInfo">
        <span>{{item[attr]}}</span>
        <div class="sortable-tree-children">
          <sortable-tree :data="item.children" v-if="item.children && item.children.length"></sortable-tree>
        </div>
      </sortable-tree-item>
    </div>
  </div>
</template>

<script>
import SortableTreeItem from './SortableTreeItem.vue'
export default {
  name: 'SortableTree',
  props: {
    data: {
      type: Array
    },
    attr: {
      type: String,
      default: 'name'
    },
    dragInfo: {
      type: Object,
      default: () => {
        return {
          vm: null,
          data: null
        }
      }
    }
  },
  data () {
    return {
    }
  },
  methods: {
  },
  components: {
    [SortableTreeItem.name]: SortableTreeItem
  }
}
</script>

<style scoped>
.sortable-tree {
  display: list-item;
  list-style: none;
}

.sortable-tree > span {
  border-bottom: 1px dashed grey;
}

.sortable-tree .sortable-tree-children {
  margin-left: 2em;
}

</style>
