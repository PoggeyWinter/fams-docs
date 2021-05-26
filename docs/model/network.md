---
sidebar_position: 3
---

# Network

A network is a series of [pipes](/docs/model/Pipe).

## Properties

| Property | Unit | Default | Notes         |
| -------- | ---- | ------- | ------------- |
| `name`   | -    | network |               |
| `nodes`  |      | `[]`    | `<Array>Node` |
| `pipes`  |      | `[]`    | `<Array>Pipe` |

### Constructor parameters

```js
interface INetwork {
  name?: string
  nodes?: Node[]
  pipes?: Pipe[]
}
```

## Methods

### addNode()

Creates a new `Node` and appends it to `nodes`. Accepts the same inputs as the `Node` constructor.

```js {2}
addNode(props: INode = {}) {
  const name = props.name || `${this.name}-N${this.nodes.length}`
  props.name = name
  const n = new Node(props)
  this.nodes.push(n)
  return n
}
```

The default name for the created node is based on the name of the network and its position in the list of nodes.

### addPipe()

Creates a new `Pipe` and appends it to the list of `pipes`. Accepts the same inputs as the `Pipe` constructor.

```js {2,4}
addPipe(props: IPipe = {}) {
  const name = props.name || `${this.name}-P${this.pipes.length}`
  props.name = name
  if (this.nodes.length) props.source = this.nodes[this.nodes.length - 1]
  const p = new Pipe(props)
  this.pipes.push(p)
  this.nodes.push(p.destination)
  return p
}
```

The default name for the created pipe is based on the name of the network and its position in the list of pipes.

If the network has nodes already, the last node is used as the source of the new pipe.

### validate()

Checks that the network's nodes and pipes meet certain conditions.

```js
validate() {
  if (!this.nodes.length) throw `Network (${this.name}) has no nodes`

  if (!this.pipes.length) throw `Network (${this.name}) has no pipes`

  const connections = this.pipes.map((pipe) => [
    pipe.source,
    pipe.destination,
  ])

  const nodesWithConnections = connections.flat()

  for (let node of this.nodes) {
  if (!nodesWithConnections.includes(node))
    throw `node (${node.name}) has missing connections`
  }
  return true
}
```
