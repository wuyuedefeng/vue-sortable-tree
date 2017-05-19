<template>
  <div>
    <div v-for="item in data">
      <sortable-tree-item :item="item" :attr="attr" :dragInfo="dragInfo">
        <div class="sortable-tree-item" :class="{'left-border': item.children && item.children.length}">
          <span>{{item[attr]}}</span>
          <div class="sortable-tree-item-children">
            <sortable-tree :data="item.children" v-if="item.children && item.children.length"></sortable-tree>
          </div>
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

<style lang="scss" scoped>
.sortable-tree-item {
  position: relative;
  display: list-item;
  list-style: none;
  padding-left: 10px;

  &:before {
    border-left: 1px solid #999;
    content: '\f147';
    position: absolute;
    left: -8px;
  }

  & > span {
    border-bottom: 1px dashed grey;
  }

  .sortable-tree-item-children {
    margin-left: 2em;
  }
}

</style>
