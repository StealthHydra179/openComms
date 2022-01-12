# OpenComms Server Spec
Can be implied from [Client Spec](../client/README.md)

If a server does not support something it should return
```json
{
    "error":501,
    "description":"not implemented"
}
```
in which case the client should try an alternative method if possible.