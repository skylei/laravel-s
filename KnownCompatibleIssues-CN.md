# 已知的兼容性问题

## 使用包 [jenssegers/agent](https://github.com/jenssegers/agent)
> [监听系统事件](https://github.com/hhxsv5/laravel-s/blob/master/README-CN.md#%E7%B3%BB%E7%BB%9F%E4%BA%8B%E4%BB%B6)

```PHP
// 重置Agent
\Event::listen('laravels.received_request', function (\Illuminate\Http\Request $req, $app) {
    $app->agent->setHttpHeaders($req->server->all());
    $app->agent->setUserAgent();
});
```

## 不能使用这些函数

- `flush`/`ob_flush`/`ob_end_flush`/`ob_implicit_flush`：`swoole_http_response`不支持`flush`。

- `exit()`/`die()`: 将导致Worker/Task/Process进程立即退出，建议通过抛异常跳出函数调用栈，[Swoole文档](https://wiki.swoole.com/wiki/page/501.html)。

- `header()`/`setcookie()`/`http_response_code()`：HTTP响应只能通过`swoole_http_response`对象。

## 不能使用的全局变量

- `$_SESSION`