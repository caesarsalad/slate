# Errors

<aside class="notice">
Runo Wallet API uses classical HTTP status code. Also it send details of error and error code. 
</aside>

| Parameter        | Type    | Description          |
| :--------------: | :----:  | :----:               |
| code             | integer | Type of code         |
| error            | string  | Description of error |


```json
{
    "code": 1005,
    "error": "You are not authorized to operate with this wallet."
}
```

The Runo Wallet API uses the following error codes:

Error Code | Meaning
---------- | -------
`1001` | JSON Is Not Valid.
`1002` | JSON Key Not Found.
`1003` | URL Error.
`1004` | Password is Wrong.
`1005` | Wallet Unauthorized.
`1006` | Webhook Exists.
`1007` | Label Exists.
`1008` | Query Parameter Limit Is Over.
`1009` | Query Parameter Error.
`1010` | Incorrect Address
`1011` | Tracking Key Not Found.
`1012` | Webhook URL Doesn't Exists.
`1013` | Balance is not sufficient.
`1014` | Tx Hash couldn't found on the network
