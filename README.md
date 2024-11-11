# TrieMap Library for Go

TrieMap is a Golang library providing an efficient Trie-based structure that 
maps IP address prefixes (netip.Prefix) to values of a generic type V. This 
library is designed to efficiently manage and query CIDR-style prefix mappings, 
allowing for quick retrieval of values based on IP address matches. The library 
supports IPv4 and IPv6 prefixes and is optimized for concurrent usage with 
read-write locking.

## Features

* **Efficient IP Prefix Matching**: Quickly match an IP address to its 
  longest-prefix match in the Trie and retrieve the associated value.
* **Generic Value Storage**: Store values of any comparable type V, reducing
  memory usage by indexing values with integer keys.
* **Thread-safe Operations**: Supports concurrent access with read-write locks,
  ensuring safety in multi-threaded environments.
* **Automatic Pruning**: Automatically removes unused nodes when values or
  prefixes are removed, optimizing memory usage.
* **IPv4 and IPv6 Support**: Distinguishes between IPv4 and IPv6 prefixes,
  maintaining separate root nodes for each address family.

## Installation

```bash
go get github.com/dpeckett/triemap
```

## Usage

### Creating a TrieMap

```go
// Create a new TrieMap for storing values of type `string`
trie := triemap.New[string]()
```

### Inserting a Prefix and Value

```go
import "net/netip"

prefix := netip.MustParsePrefix("192.168.1.0/24")
trie.Insert(prefix, "my-value")
```

### Retrieving a Value by IP Address

```go
addr := netip.MustParseAddr("192.168.1.45")
value, contains := trie.Get(addr)
if contains {
    fmt.Println("Found value:", value)
} else {
    fmt.Println("No match found for address")
}
```

### Removing a Prefix

```go
prefix := netip.MustParsePrefix("192.168.1.0/24")
removed := trie.Remove(prefix)
if removed {
    fmt.Println("Prefix removed successfully")
} else {
    fmt.Println("Prefix not found")
}
```

### Removing All Prefixes Associated with a Value

```go
trie.RemoveValue("my-value")
```

### Checking if the TrieMap is Empty

```go
isEmpty := trie.Empty()
```

## License

This library is licensed under the Mozilla Public License, v2.0 with portions 
based on code from the Kubernetes project under the Apache License, Version 2.0.

## Contributing

Contributions are welcome! Please submit issues or pull requests for any bugs, 
features, or improvements.