# Network Topology API (JSON)

Define the shape of your network - what nodes are present, what programs they each run, and how they're connected,
by coding your topology.

### The basics

Your topology should be a map/dictionary with at least one of the keys, `single_nodes` and `node_groups`. The values for
each of these will be a list of dictionaries, each representing a node or a group of nodes respectively.

---

```json
{
  "single_nodes": [ ... ],
  "node_groups": [ ... ]
}
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
{
  "single_nodes": [
    {
      "nid": "alice",
      "program": "hello_whirled"
    },
    {
      "nid": "bob",
      "program": "hello_whirled",
      "connections": [
        "alice",
        "charlie"
      ]
    },
    {
      "nid": "charlie",
      "program": "hello_whirled"
    }
  ]
}
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

You can also create groups of automatically-generated nodes, arranged in the common topologies. The node IDs _`nid`s_
of automatically-generated nodes will be in the form `<PREFIX><COUNTER><SUFFIX>`. `<COUNTER>` is an incrementing
integer, and `<PREFIX>` and `<SUFFIX>` are determined by you.

Here's an example of a `node_groups` list:

---

```yaml
{
  "node_groups": [
    {
      "type": "ring",
      "number_nodes": 4,
      "program": "ring_program",
      "nid_prefix": "r",
      "nid_suffix": "-ing",
      "nid_starting_number": 3,
      "nid_number_increment": 3,
      "connections": [
        {
          "from": "r3-ing",
          "to": "line1"
        }
      ]
    },
    {
      "type": "line",
      "number_nodes": 3,
      "program": "program_for_line_nodes",
      "nid_prefix": "line"
    }
  ]
}
```

---

This gives the following topology:

---

```text
 _______      _______      _______
| line0 |----| line1 |----| line2 |
 ¯¯¯¯¯¯¯      ¯¯¯¯¯¯¯      ¯¯¯¯¯¯¯
                 |
                 |
              ________    ________
             | r3-ing |--| r6-ing |
              ¯¯¯¯¯¯¯¯    ¯¯¯¯¯¯¯¯
                |              |
              ________    ________
             | r12-ing |--| r9-ing |
              ¯¯¯¯¯¯¯¯    ¯¯¯¯¯¯¯¯
```

---

#### Line, ring and fully-connected

Node groups for these topologies have the following keys:
- `type` _string_ - the type of standard network topology. It must be one of `ring`, `line` or `fully_connected`.
- `number_nodes` _integer_ - the number of nodes in this topology.
- `program` _string_ - the name of the program that the nodes in this topology should run.
- `nid_prefix` _string_ - the prefix for the generated `nid`s.
- `nid_suffix` _string (optional)_ - the suffix for the generated `nid`s. The default suffix is the empty string.
- `nid_starting_number` _integer (optional)_ - the first number of the counter for the generated `nid`s. The default
  starting number is 0.
- `nid_number_increment` _integer (optional)_ - the increment step size of the counter for the generated `nid`s. The
  default step size is 1.
- `connections` _list of dictionaries (optional)_ - extra connections from a node in this group to another node in the
  overall topology. Each dictionary in this list has two keys: `from` and `to`, which take the `nid`s of the two nodes
  to connect together.

#### Star

A star network has a hub and some number of nodes connected to that hub. A star network node group has these keys:
- `type` _string_ - `star`.
- `hub_program` _string_ - the name of the program that the hub node should run.
- `hub_nid` _string_ - the `nid` of the hub node.
- `number_hosts` _integer_ - the number of hosts in this star topology.
- `host_program` _string_ - the name of the program that the host nodes should run.
- `host_nid_prefix` _string_ - the prefix for the generated `nid`s of the host nodes.
- `host_nid_suffix` _string (optional)_ - the suffix for the generated `nid`s of the host nodes. The default suffix is
  the empty string.
- `host_nid_starting_number` _integer (optional)_ - the first number of the counter for the generated `nid`s of the
  host nodes. The default starting number is 0.
- `host_nid_number_increment` _integer (optional)_ - the increment step size of the counter for the generated `nid`s
  of the host nodes. The default step size is 1.
- `connections` _list of dictionaries (optional)_ - extra connections from a node in this star topology to another node
  in the overall topology. Each dictionary in this list has two keys: `from` and `to`, which take the `nid`s of the two
  nodes to connect together.

#### Tree (or _hierarchical_)

A tree network has several layers of nodes. At the top level is the root. At the next level is the children of the root,
and in the next level, the grandchildren of the root, and so on. A group of nodes in a tree network has the following
keys:
- `type` _string_ - `tree`.
- `number_levels` _integer_ - the number of node levels.
- `number_children` _integer_ - the number of children (in the next level) that each node has.
- `programs` _ordered list of strings_ - the names of the programs that nodes on each level should run, starting from
  the first level (root).
- `nid_prefixes` _ordered list of strings_ - the prefixes for the generated `nid`s of the nodes on each level.
- `nid_suffixes` _ordered list of strings (optional)_ - the suffixes for the generated `nid`s of the nodes on each
  level. The default suffix is the empty string.
- `nid_starting_numbers` _ordered list of integers (optional)_ - the first numbers of the counters for the generated
  `nid`s of the nodes on each level. The default starting number is 0.
- `nid_number_increments` _ordered list of integers (optional)_ - the increment step sizes of the counters for the
  generated `nid`s of the nodes on each level. The default step size is 1.
- `connections` _list of dictionaries (optional)_ - extra connections from a node in this tree topology to another node
  in the overall topology. Each dictionary in this list has two keys: `from` and `to`, which take the `nid`s of the two
  nodes to connect together.
