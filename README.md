# CoE 151 Machine Problem 1 Chat Server Network Protocol

## Compiling the RFC File
You can either install the official xml2rfc [here](https://xml2rfc.tools.ietf.org/), or install the python module xml2rfc by running the following:

`
pip install -r requirements.txt
`

If using the python module, simply run the following:

`
xml2rfc coe-151-mp1-protocol.xml -o protocol/README.txt
`

## Models
### Server-Client Model
    Server
        Client
        Client
        Client
### Peer-Peer Model (?)
    Basically choose if ur gonna run on server mode or client mode lmao.

# Server-Client Messages and Format
## Serverbound Data
Messages sent to the server
###   RequestUsername
The client requests the server to assign a username for him/her.
###  RequestUserInfo
The client requests the information of a user in the chat server.
###   RequestLocalTime
The client requests the local time of the server.
###   SetUsername
The client requests the server to change his/her username.
###  WhisperToUser
Private Message. Send a message to a user that only the sender, receiver and the server can read.
###  SendChat
Message to be read by everyone online in the chat server.
###    RequestOnlineList
Request the usernames of the people in the server.
###    Login
Handshake message to the server sent by the client.
### Disconnect
Informs the server that the client is disconnecting. Adds robustness in the application layer for terminating connections.
### KeepAlive
Regular message sent to the server to ensure connection still exists. Only necessary however on cases where the client is quiet.

## Custom Serverbound Data
###   SetAdmin
An admin user grants admin priveleges to another user.
###   RequestKickUser
An admin user kicks a user from the server. Only for admins.
###   RequestIgnoreUser
Requests server to not send messages from a certain user to the client.
###   RequestMuteUser
Disables the chat abilities of a certain user. Only for admins.
###   RequestUnmuteUser
Re-enables the chat abilities of a certain user. Only for admins.

## Clientbound Data
### AssignUsername
Response to either RequestUsername or SetUsername message sent by the client. Assigns a username and sends it to everyone in the chat server, if granted. The server resends the current username to the requesting client if rejected.

###   ProvideUserInfo
Sends the details of a user to the requesting client. *Should we inform the user that someone asked for his details?*

###  SendLocalTime
Sends the local time of the server to the requesting client.

###   WhisperFromUser
Sends a whisper message to the user targetted by another user.

###   SendChatFromUser
General chat send by a user for everyone.

### SendOnlineList
Server responds the list of online people in the server.

### AnswerLogin
Handshake message. Answers the client if accepted or rejected. Reason for rejecting is also embedded in the message. 

###  KickUser
Sends a kick message to the client. Disconnects the client forcibly afterwards.

### MuteUser
Sends a mute message to the client to inform that he is muted. Will ignore all SendChat and WhisperToUser message from that client until unmuted accordingly.

### UnmuteUser
Sends an unmute message to the client to inform that he is unmuted. Will process all SendChat and WhisperToUser messages again.

# Peer-Peer Messages and Format
Placeholder