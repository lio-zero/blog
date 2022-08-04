# Vue 递归组件

> [SFC](https://vuejs.org/guide/scaling-up/sfc.html) 可以通过其文件名隐式引用自身。

即组件通过组件名引用自身，这种情况就是[递归组件](https://vuejs.org/api/sfc-script-setup.html#using-components)。

一个简单的示例：

```html
<!-- index.vue -->
<template>
  <TreeItem :treeData="treeData"></TreeItem>
</template>

<script setup>
  import TreeItem from './TreeItem.vue'

  const treeData = [
    {
      label: 'Level one 1',
      children: [
        {
          label: 'Level two 1-1',
          children: [
            {
              label: 'Level three 1-1-1'
            }
          ]
        }
      ]
    },
    {
      label: 'Level one 2',
      children: [
        {
          label: 'Level two 2-1',
          children: [
            {
              label: 'Level three 2-1-1'
            }
          ]
        },
        {
          label: 'Level two 2-2',
          children: [
            {
              label: 'Level three 2-2-1'
            }
          ]
        }
      ]
    },
    {
      label: 'Level one 3',
      children: [
        {
          label: 'Level two 3-1',
          children: [
            {
              label: 'Level three 3-1-1'
            }
          ]
        },
        {
          label: 'Level two 3-2',
          children: [
            {
              label: 'Level three 3-2-1'
            }
          ]
        }
      ]
    }
  ]
</script>
```

```html
<!-- TreeItem.vue -->
<template>
  <div class="tree">
    <div v-for="tree in treeData" :key="tree.label">
      <div class="tree-node__label">{{ tree.label }}</div>
      <div
        v-if="tree.children && tree.children.length"
        class="tree-node__children"
      >
        <!-- 递归调用自身 -->
        <TreeItem :treeData="tree.children" />
      </div>
    </div>
  </div>
</template>

<script setup>
  const props = defineProps({
    treeData: {
      type: Array,
      default: () => []
    }
  })
</script>
```

上面的示例中，我们使用 `TreeItem` 组件，引用了自身。

我们知道，在使用递归的时候，我们需要给予递归正确的终止条件，以确保不会导致无限循环。Vue 递归组件也不例外，我们需要使用 `v-if` 指令进行判断。

在 Vue3 中，它会通过其文件名隐式引用自身。而在 Vue2 中，你需要指定一个 `name` 选项。

```js
export default {
  name: 'TreeItem'
}
```

还需要注意，它的优先级低于导入的组件。如果您的命名导入与组件的推断名称冲突，您可以为导入设置别名：

```js
import { TreeItem as TreeItemChild } from './components'
```

递归组件应用场景相对较少，常用于 Tree、Menu 这类组件，它们的节点往往包含子节点，子节点结构和父节点往往是相同的。这类组件的数据往往也是树形结构。
