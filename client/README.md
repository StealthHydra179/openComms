# OpenComms Client Spec
All data is sent in a json format (converted to string).

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

Server Responses:
**WIP**

## Login
Retrieving a client token. Client tokens expire after 1 hour of inactivity or 24 hours of being created, whichever comes first.

Request format:
| Name     | Type   | Value             |
| -------- | ------ | ----------------- |
| type     | String | "login"           |
| username | String | username or email | 
| password | String | password          |
| time     | String | unix timestamp    |

**WIP**

## Signout
Closing a client token
**WIP**

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
    "token": "Edji83djUVyeufiiuchushd",
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
**WIP**

## Leaving a server
**NOT DEFINED YET**

## Messaging

### Sending \[Mandatory]
When sending a message send it in a json format. The server will respond with 200 if it goes through.


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
    "token": "Edji83djUVyeufiiuchushd",
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
When recieving a message in json format the server will send a message

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