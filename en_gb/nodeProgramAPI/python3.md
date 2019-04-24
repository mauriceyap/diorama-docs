You need to implement the *main* function of the node, which will be called when the node is started. This function will
be passed five positional arguments:

```python
def your_main_function_name(peer_nids, my_nid, send, receive, storage):
    ...
    # your code
    ...
```

- **`peer_nids`** *list(str)*
  - A list of the node IDs (nids) of all nodes that this node is connected to.
- **`my_nid`** *str*
  - The node ID of this node.
- **`send`** *method*
  - Sends a message to a connected node.
  - `send(message, recipient_nid)`
    - `message` *bytes* - a bytes object to be sent
    - `recipient_nid` *str* - the node ID of the node to whom the message is being sent.
- **`receive`** *generator*
  - A blocking generator of messages received, which can be iterated over.
  - Yields a tuple of `(message, sender_nid)`, where:
    - `message` *bytes* is the received bytes object
    - `sender_nid` *str* is the node ID of the sender.
- **`storage`** *Storage object*
  - A persistent static key-value pair data store, which can be used, for example, to recover data after a node failure.
  - **`storage.get(key)`** returns the value associated with the given key, or `None` if there isn't one.
    - `key` *str* - the key to get.
  - **`storage.get_all()`** *dict* returns a dictionary of all the key-value pairs stored.
  - **`storage.put(key, value)`** stores the given key-value pair. If a pair with the same key exists, this replaces it.
    - `key` *str* - the key.
    - `value` *str, int, float, bool, list or dict*
  - **`storage.remove(key)`** removes the key-value pair with the given key.
    - `key` *str* - the key of the pair to remove.
  - **`storage.contains_key(key)`** *bool* returns whether or not a pair with the given key is present in the store.
    - `key` *str* - the key to look for.
  - **`storage.clear()`** removes all key-value pairs from the store.
  - **`storage.size()`** *int* returns the number of key-value pairs in the store.
