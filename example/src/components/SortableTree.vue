<template>
  <div class="sortable-tree">
    <span>o {{data[attr]}}</span>
    <ul v-if="hasChildren(data)">
      <li v-for="item in data.children" :class="{'parent-li': hasChildren(item)}">
        <sortable-tree-item :item="item" :attr="attr" :dragInfo="dragInfo">
          <sortable-tree :data="item"></sortable-tree>
        </sortable-tree-item>
      </li>
    </ul>
  </div>
</template>

<script>
import SortableTreeItem from './SortableTreeItem.vue'
export default {
  name: 'SortableTree',
  props: {
    data: {
      type: Object
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
    hasChildren (item) {
      return item.children && item.children.length
    }
  },
  components: {
    [SortableTreeItem.name]: SortableTreeItem
  }
}
</script>

<style lang="scss" scoped>
.sortable-tree {
  font-size: 16px;

  ul, li {
    margin: 0;
    padding: 0;
  }

  ul {
    position: relative;
    display: list-item;
    list-style: none;
    &:empty {
      width: 0;
      height: 0;
    }
  }

  li {
    position: relative;
    padding: 10px 0 0 24px;
  }
}
/* 位置相关 */
.sortable-tree {
  li {
    position: relative;

    &:before, &:after {
      position: absolute;
      content: '';
    }
    &:before {
      width: 24px;
      height: 100%;
      left: 0;
      top: -10px;
      /*background: red;*/
      border-left: 1px solid #999;
    }
    &:after {
      width: 22px;
      height: 20px;
      top: 22px;
      left: 0;
      border-top: 1px solid #999;
    }
    &.parent-li:last-child:before {
      width: 24px;
      height: 32px; // 32为1个li的高度
      left: 0;
      top: -10px;
      border-left: 1px solid #999;
    }
  }
}
</style>
