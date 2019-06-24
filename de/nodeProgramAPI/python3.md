**Leider haben wir diese Dokumentation noch nicht auf Deutsch. Hoffentlich wird es bald verfügbar sein!**

# Python 3 Node Program API

You need to implement the _main_ function of the node, which will be called when the node is started. This function will
be passed five positional arguments:

---

```python
def your_main_function_name(peer_nids, my_nid, send, receive, storage):
    ...
    # your code
    ...
```

---

- **`peer_nids`** _list(str)_
  - A list of the node IDs (nids) of all nodes that this node is connected to.
- **`my_nid`** _str_
  - The node ID of this node.
- **`send`** _method_
  - Sends a message to a connected node.
  - `send(message, recipient_nid)`
    - `message` _bytes_ - a bytes object to be sent
    - `recipient_nid` _str_ - the node ID of the node to whom the message is being sent.
- **`receive`** _generator_
  - A blocking generator of messages received, which can be iterated over.
  - Yields a tuple of `(message, sender_nid)`, where:
    - `message` _bytes_ is the received bytes object
    - `sender_nid` _str_ is the node ID of the sender.
- **`storage`** _Storage object_
  - A persistent static key-value pair data store, which can be used, for example, to recover data after a node failure.
  - **`storage.get(key)`** returns the value associated with the given key, or `None` if there isn't one.
    - `key` _str_ - the key to get.
  - **`storage.get_all()`** _dict_ returns a dictionary of all the key-value pairs stored.
  - **`storage.put(key, value)`** stores the given key-value pair. If a pair with the same key exists, this replaces it.
    - `key` _str_ - the key.
    - `value` _str, int, float, bool, list or dict_
  - **`storage.remove(key)`** removes the key-value pair with the given key.
    - `key` _str_ - the key of the pair to remove.
  - **`storage.contains_key(key)`** _bool_ returns whether or not a pair with the given key is present in the store.
    - `key` _str_ - the key to look for.
  - **`storage.clear()`** removes all key-value pairs from the store.
  - **`storage.size()`** _int_ returns the number of key-value pairs in the store.
