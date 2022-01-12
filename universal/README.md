# Universal Information
Things that pertain to both client and server

## Message IDs
Message IDs pertain to channels specifically, and channel ids are per server, so referencing a message should be done like "serverid:channelid:messageid".

Message IDs incremement from 0, with 0 being the first message.

## User Tokens
Used to verify identity and make sure the user is who they say they are duing a session. Currently implementation defined (on what constitutes a token) one is generated on login.