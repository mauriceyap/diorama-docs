Define the shape of your network - what nodes are present, what programs they each run, and how they're connected,
by coding your topology.

### The basics

Your topology should be a map/dictionary with at least one of the keys, `single_nodes` and `node_groups`. The values for
each of these will be a list of dictionaries, each representing a node or a group of nodes respectively.

---

```yaml
single_nodes:
  - ...
    ...
  - ...
    ...
node_groups:
  - ...
    ...
```

---

### Single nodes

Each node in `single_nodes` has the following keys:

- `nid` _string_ the ID of the node (determined by you)
- `program` _string_ the name of the program that this node should run
- `connections` _list of strings (optional)_ list of node IDs that this node is connected to. _All connections are
  two-way, so if you only need to define each connected once._

This is an example of a `single_nodes` list:

---

```yaml
single_nodes:
  - nid: alice
    program: hello_whirled
  - nid: bob
    program: hello_whirled
    connections:
      - alice
      - charlie
  - nid: charlie
    program: hello_whirled
```

---

Here, we have three creatively-named nodes, `alice`, `bob` and `charlie`, who are all running the `hello_whirled`
program. `alice` is connected to `bob`, who is connected to `charlie`. We have defined both these connections in the
representation of `bob`, but we equally could have moved defined each connection in `alice` and `charlie` instead.

---

```text
 _______      _____      _________
| alice |----| bob |----| charlie |
 ¯¯¯¯¯¯¯      ¯¯¯¯¯      ¯¯¯¯¯¯¯¯¯
```

---

### Node Groups

TODO

#### Star

TODO

#### Tree

TODO

TODO other node groups
