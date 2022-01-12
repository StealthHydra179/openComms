# OpenComms Client Spec
All data is sent in a json format (converted to string).

> *More specific example requests will be added in a later version*

## Connecting
Websocket

## Signup
Creating an account. Does not retrieve a client token.

Request format:
| Name     | Type   | Value             |
| -------- | ------ | ----------------- |
| type     | String | "signup"          |
| username | String | username          |
| email    | String | email             | 
| password | String | password          |
| time     | String | unix timestamp    |

```json
{
    "type": "signup",
    "username": "ExampleUser",
    "email": "email@example.com",
    "password": "Example Password",
    "time": "1641138383"
}
```

Server Response:
| Name        | Type    | Value              |
| ----------- | ------- | ------------------ |
| type        | String  | "signup"           |
| code        | Integer | Server return code |
| description | String  | more info          |
| time        | String  | unix timestamp     |

```json
{
    "type": "signup",
    "code": 200,
    "description": "success",
    "time": "1641138383"
}
```


## Login
Retrieving a client token. Client tokens expire after 1 hour of inactivity or 24 hours of being created, whichever comes first.

Request format:
| Name     | Type   | Value             |
| -------- | ------ | ----------------- |
| type     | String | "login"           |
| username | String | username or email | 
| password | String | password          |
| time     | String | unix timestamp    |

```json
{
    "type": "login",
    "username": "ExampleUser",
    "password": "Example Password",
    "time": "1641138383"
}
```

Server Response:
| Name        | Type    | Value              |
| ----------- | ------- | ------------------ |
| type        | String  | "login"            |
| code        | Integer | Server return code |
| description | String  | more info          |
| token       | String  | token              |
| time        | String  | unix timestamp     |

```json
{
    "type": "login",
    "code": 200,
    "description": "success",
    "token": "example_token",
    "time": "1641138383"
}
```

## Signout
Closing a client token

Request format:
| Name     | Type   | Value             |
| -------- | ------ | ----------------- |
| type     | String | "signout"         |
| token    | String | user token        |
| time     | String | unix timestamp    |

```json
{
    "type": "signout",
    "token": "example_token",
    "time": "1641138383"
}
```
Server response:
| Name     | Type   | Value                |
| -------- | ------ | -------------------- |
| type     | String | "signout"            |
| code     | String | server response code |
| time     | String | unix timestamp       |

```json
{
    "type": "signout",
    "code": 200,
    "time": "1641138383"
}
```

## Joining a Server

### Public Server \[Mandatory]
Public servers do not require passwords.

| Name      | Type   | Value                 |
| --------- | ------ | --------------------- |
| token     | String | Issued Client Token   |
| type      | String | "server_join"         |
| server    | String | Hexidecimal Server ID |
| invite    | String | Hexidecimal Invite ID |
| timestamp | String | Unix Timestamp        |

```json
{
    "token": "example_token",
    "type": "server_join",
    "server": "0x0",
    "invite": "0x0",
    "timestamp": "1641138383"
}
```
Server will return the status code, and if successfull it will also return the server layout and default channel id.

| Name           | Type         | Value                                           |
| -------------- | ------------ | ----------------------------------------------- |
| type           | String       | "server_join"                                   |
| code           | Integer      | Server return code                              |
| serverContents | json(String) | If server returns 200 serverContents will exist |
| defaultChannel | String       | Hexidecimal Channel ID                          |

Server Contents:
| Name       | Type         | Value            |
| ---------- | ------------ | ---------------- |
| serverName | String       | Name of server   |
| serverID   | String       | Hex ID of server |
| channels   | json(String) | List of channels |

Channels:
| Name      | Type         | Value              |
| --------- | ------------ | ------------------ |
| channelID | json(String) | Info about channel |

Channel Information:
| Name        | Type   | Value                          |
| ----------- | ------ | ------------------------------ |
| name        | String | Name of channel                |
| description | String | The description of the channel |

Example:
```json
{
    "type": "server_join",
    "code": 200,
    "serverContents": {
        "serverName": "Example server",
        "serverID": "0x0",
        "channels": {
            "0x0": {
                "name": "Example Channel",
                "description": "Simple example channel"
            }
        }
    },
    "defaultChannel": "0x0"
}
```

### Private Server \[Recommended]
The same as joining a public server except the sent packet also contains `password` wich has type `String` that contains the password to the server.

## Leaving a server
**NOT DEFINED YET**

## Messaging

### Sending \[Mandatory]
When sending a message the server will respond with 200 if it goes through.


| Name      | Type        | Value                  |
| --------- | ----------- | ---------------------- |
| token     | String      | Issued Client Token    |
| type      | String      | "sent_message"         |
| server    | String      | Hexidecimal Server ID  |
| channel   | String      | Hexidscimal Channel ID |
| content   | String      | Message Content        |
| timestamp | String      | Unix Timestamp         |

Example Message 
```json
{
    "token": "example_token",
    "type": "sent_message",
    "server": "0x0",
    "channel": "0x0",
    "content": "example message",
    "timestamp": "1641138383"
}
```

Response from server will be a code along with the type and content of the sent data.

| Name      | Type       | Value                     |
| --------- | ---------- | ------------------------- |
| type      | String     | "sent_message"            |
| code      | Integer    | http error code           |
| timestamp | String     | Unix Timestamp of message |
| id        | String     | Hexidecimal ID of message |

Example Success Response
```json
{
    "type": "sent_message",
    "code": 200,
    "content": "example message",
    "timestamp": "1641138383",
    "id": "0x0"
}
```

### Recieving \[Mandatory]
When recieving a message the server will send a message

| Name       | Type        | Value                  |
| ---------- | ----------- | ---------------------- |
| type       | String      | "recieved_message"     |
| server     | String      | Hexidecimal Server ID  |
| channel    | String      | Hexidscimal Channel ID |
| content    | String      | Message Content        |
| timestamp  | String      | Unix Timestamp         |
| id         | String      | Hexidecimal Message ID |
| sender     | String      | Hexidecimal Sender ID  |

```json
{
    "type": "recieved_message",
    "server": "0x0",
    "channel": "0x0",
    "content": "example message",
    "timestamp": "1641138383",
    "id": "0x0",
    "sender": "0x0"
}
```

No response is necessary

### Requesting a message \[Mandatory]
A client may need to request a unloaded message for display purposes, in which the client will request
| Name       | Type        | Value                         |
| ---------- | ----------- | ----------------------------- |
| token      | String      | Issued client token           |
| type       | String      | "request_message"             |
| server     | String      | Hexidecimal Server ID         |
| channel    | String      | Hexidscimal Channel ID        |
| message    | String      | ID of message to be retrieved |

```json
{
    "token": "example_token",
    "type": "request_message",
    "server": "0x0",
    "channel": "0x0",
    "message": "0x0"
}
```


### Requesting a series of messages \[Recommended]
**[WIP]**

When a client loads up, often they need to load a large amount of messages at once
| Name       | Type        | Value                  |
| ---------- | ----------- | ---------------------- |
| token      | String      | Issued client token    |
| type       | String      | "request_message_list" |
| server     | String      | Hexidecimal Server ID  |
| channel    | String      | Hexidscimal Channel ID |
| newest     | String      | ID of latest message   |
| oldest     | String      | ID of oldest message   |

`oldest` is always a smaller id than `newest` since newer messages have larger ids.

```json
{
    "token": "example_token",
    "type": "request_message",
    "server": "0x0",
    "channel": "0x0",
    "newest": "0xF",
    "oldest": "0x0"
}
```