# AI Fix Notes

Session: seq-1777372409417-ao7dyl3vl
Repository: Nisha290403/C-C-

- [1] (critical) client_server/tcp_full_duplex_server.c: Fork-based TCP server code must be treated as security-sensitive: validate all socket reads/writes, enforce message length limits, and handle partial sends/receives. Without strict bounds and protocol framing, the server may be vulnerable to buffer overflow, denial of service, or data corruption.
- [2] (critical) data_structures/binary_trees/red_black_tree.c: newNode() allocates memory and initializes fields but does not return the created node. This is a serious bug that will lead to undefined behavior whenever the caller uses the returned value.
- [3] (high) client_server/tcp_full_duplex_client.c: The design uses fork-based concurrent send/receive loops. This pattern can introduce process-management bugs, zombie processes, and inconsistent shutdown behavior if SIGCHLD handling, parent/child termination, and socket closure are not carefully implemented. Ensure both processes handle EOF, interrupts, and errors cleanly, and that the socket is closed exactly once per process when appropriate.
- [4] (high) client_server/tcp_full_duplex_server.c: Infinite-loop duplex communication with fork requires careful child/process cleanup and signal handling. If zombie processes, dead sockets, or SIGPIPE are not handled, the server can leak resources or terminate unexpectedly.
- [5] (high) data_structures/array/carray_tests.c: No visible null-check or allocation-failure handling after getCArray(10). If allocation fails, dereferencing array->size will crash. Add a defensive check before use.

