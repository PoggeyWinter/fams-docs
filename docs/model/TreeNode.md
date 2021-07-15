---
sidebar_position: 1
---

# TreeNode

This class provides the functionality for different entities to connect to one another.

They can be recursively stored as child nodes in the parent's `sources` array, which allows for the data to be structured as a [tree](<https://en.wikipedia.org/wiki/Tree_(data_structure)>).  
This is important because we have to navigate through the network using post-order traversal to ensure that each calculation happens in the correct order.

```js
interface ITNode {
  value?: number
  name: string
}

export default class TreeNode {
  value: number
  name: string
  sources: TreeNode[]
  destination: TreeNode | null

  constructor(props: ITNode = { name: 'treenode' }) {
    this.value = props.value || 0
    this.name = props.name
    this.sources = []
    this.destination = null
  }

  addSource(node: TreeNode) {
    node.destination = this
    this.sources.push(node)
  }
}
```
