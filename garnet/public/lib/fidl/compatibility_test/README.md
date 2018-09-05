<!-- # FIDL compatibility test -->
# FIDL兼容性测试
---

[*英文原文快照*](https://github.com/fuchsia-mirror/garnet/blob/a01c3dcf48592790417b2f8bf4576675957399b7/public/lib/fidl/compatibility_test/README.md)

---
<!-- An integration test for compatability of different FIDL bindings. -->
针对不同FIDL绑定的兼容性集成测试。

<!-- The test runner is at `//garnet/bin/fidl_compatibility_test` and
`//topaz/bin/fidl_compatibility_test` and can be invoked on device with: -->
测试运行器(test runner)位于`//garnet/bin/fidl_compatibility_test`和`//topaz/bin/fidl_compatibility_test`，并可以在设备上调用：
```sh
run /pkgfs/packages/fidl_compatibility_test/0/test/fidl_compatibility_test
```

<!-- The version in topaz tests more languages than the version in garnet. -->
topaz版本中所测试的语言比garnet版本中的更多。

<!-- The basic logic is along the lines of: -->
其基本逻辑类似于：
```python
servers = ['go_server', 'cc_server', ...]

for proxy_name in servers:
  for server_name in servers:
    proxy = <connect to proxy>
    struct = <construct complicated struct>
    resp = proxy.EchoStruct(struct, server_name)
    assert_equal(struct, resp)
```

<!-- Servers should implement the service defined in
[compatibility_test_service.fidl](compatibility_test_service.fidl) with logic
along the lines of: -->

服务端应该使用类似以下的逻辑实现[compatibility_test_service.fidl](compatibility_test_service.fidl)中定义的服务：

<!-- ```python
def EchoStruct(
    Struct value, string forward_to_server, EchoStructCallback callback):
  if value.forward_to_server:
    other_server = <start server with LaunchPad>
    # set forward_to_server to "" to prevent recursion
    other_server.EchoStruct(value, "", callback)
  else:
    callback(value)
``` -->
```python
def EchoStruct(
    Struct value, string forward_to_server, EchoStructCallback callback):
  if value.forward_to_server:
    other_server = <start server with LaunchPad>
    # 设置forward_to_server为""以防止递归
    other_server.EchoStruct(value, "", callback)
  else:
    callback(value)
```
<!-- The logic for `EchoStructNoRetVal()` is similar. Instead of waiting for a
response directly, the test waits to recieve an `EchoEvent()`. And instead of
calling the client back directly, the server sends the `EchoEvent()`. -->
`EchoStructNoRetVal()`的逻辑也是类似的。 
测试端等待接收`EchoEvent()`事件而不是直接等待响应，并且服务端发送`EchoEvent()`而不是直接回调客户端。