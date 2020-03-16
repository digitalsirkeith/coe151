



CoE 151                                                        K. Vargas
Internet-Draft                                                      EEEI
Updates: 5 (if approved)                                  March 16, 2020
Intended status: Standards Track
Expires: September 17, 2020


           Server-Client and Peer-Peer Communication Protocol
                        coe-151-mp1-protocol-05

Abstract

   This document describes the communication protocol of the server-
   client model and peer-peer model.  This will not cover rules and
   restrictions for implementations of the control logic and processing
   of user input.  Message formats are described along with their use
   cases.  Handshake / login, state transitions, and terminations are
   also described below.

Disclaimer

   The template used is from [RFC3470].  The document attempts to follow
   guidelines set in [RFC6949].

Status of This Memo

   This Internet-Draft is submitted in full conformance with the
   provisions of BCP 78 and BCP 79.

   Internet-Drafts are working documents of the Internet Engineering
   Task Force (IETF).  Note that other groups may also distribute
   working documents as Internet-Drafts.  The list of current Internet-
   Drafts is at https://datatracker.ietf.org/drafts/current/.

   Internet-Drafts are draft documents valid for a maximum of six months
   and may be updated, replaced, or obsoleted by other documents at any
   time.  It is inappropriate to use Internet-Drafts as reference
   material or to cite them other than as "work in progress."

   This Internet-Draft will expire on September 17, 2020.

Copyright Notice

   Copyright (c) 2020 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents



Vargas                 Expires September 17, 2020               [Page 1]

Internet-Draft             CoE 151 MP Protocol                March 2020


   (https://trustee.ietf.org/license-info) in effect on the date of
   publication of this document.  Please review these documents
   carefully, as they describe your rights and restrictions with respect
   to this document.

Table of Contents

   1.  Introduction  . . . . . . . . . . . . . . . . . . . . . . . .   3
   2.  Message Formats . . . . . . . . . . . . . . . . . . . . . . .   3
     2.1.  Introduction  . . . . . . . . . . . . . . . . . . . . . .   3
     2.2.  Serverbound Messages  . . . . . . . . . . . . . . . . . .   4
       2.2.1.  Login . . . . . . . . . . . . . . . . . . . . . . . .   4
       2.2.2.  SetUsername . . . . . . . . . . . . . . . . . . . . .   4
       2.2.3.  RequestUserInfo . . . . . . . . . . . . . . . . . . .   5
       2.2.4.  RequestLocalTime  . . . . . . . . . . . . . . . . . .   5
       2.2.5.  WhisperToUser . . . . . . . . . . . . . . . . . . . .   6
       2.2.6.  SendChat  . . . . . . . . . . . . . . . . . . . . . .   6
       2.2.7.  RequestOnlineList . . . . . . . . . . . . . . . . . .   7
       2.2.8.  Disconnect  . . . . . . . . . . . . . . . . . . . . .   7
       2.2.9.  KickUser  . . . . . . . . . . . . . . . . . . . . . .   7
       2.2.10. MuteUser  . . . . . . . . . . . . . . . . . . . . . .   8
       2.2.11. UnmuteUser  . . . . . . . . . . . . . . . . . . . . .   8
       2.2.12. SetAsAdmin  . . . . . . . . . . . . . . . . . . . . .   9
     2.3.  Clientbound Messages  . . . . . . . . . . . . . . . . . .   9
       2.3.1.  AssignUsername  . . . . . . . . . . . . . . . . . . .   9
       2.3.2.  SetUsername . . . . . . . . . . . . . . . . . . . . .  10
       2.3.3.  ProvideUserInfo . . . . . . . . . . . . . . . . . . .  11
       2.3.4.  SendLocalTime . . . . . . . . . . . . . . . . . . . .  12
       2.3.5.  WhisperFromUser . . . . . . . . . . . . . . . . . . .  12
       2.3.6.  SendChatFromUser  . . . . . . . . . . . . . . . . . .  13
       2.3.7.  SendOnlineList  . . . . . . . . . . . . . . . . . . .  13
       2.3.8.  KickUser  . . . . . . . . . . . . . . . . . . . . . .  14
       2.3.9.  MuteUser  . . . . . . . . . . . . . . . . . . . . . .  15
       2.3.10. UnmuteUser  . . . . . . . . . . . . . . . . . . . . .  15
       2.3.11. SetAsAdmin  . . . . . . . . . . . . . . . . . . . . .  16
       2.3.12. ServerMessage . . . . . . . . . . . . . . . . . . . .  17
       2.3.13. Disconnect  . . . . . . . . . . . . . . . . . . . . .  17
     2.4.  Peer-Peer Messages  . . . . . . . . . . . . . . . . . . .  18
       2.4.1.  Discovery . . . . . . . . . . . . . . . . . . . . . .  18
       2.4.2.  DiscoveryResponse . . . . . . . . . . . . . . . . . .  18
       2.4.3.  Handshake . . . . . . . . . . . . . . . . . . . . . .  19
       2.4.4.  HandshakeResponse . . . . . . . . . . . . . . . . . .  20
       2.4.5.  SendChat  . . . . . . . . . . . . . . . . . . . . . .  20
       2.4.6.  Whisper . . . . . . . . . . . . . . . . . . . . . . .  21
       2.4.7.  Disconnect  . . . . . . . . . . . . . . . . . . . . .  21
   3.  Server-Client Model . . . . . . . . . . . . . . . . . . . . .  22
     3.1.  States / Phases . . . . . . . . . . . . . . . . . . . . .  22
       3.1.1.  Login . . . . . . . . . . . . . . . . . . . . . . . .  22



Vargas                 Expires September 17, 2020               [Page 2]

Internet-Draft             CoE 151 MP Protocol                March 2020


     3.2.  Request-Response Behavior . . . . . . . . . . . . . . . .  22
       3.2.1.  SetUsername . . . . . . . . . . . . . . . . . . . . .  22
       3.2.2.  UserInfo  . . . . . . . . . . . . . . . . . . . . . .  23
       3.2.3.  LocalTime . . . . . . . . . . . . . . . . . . . . . .  23
       3.2.4.  Whisper . . . . . . . . . . . . . . . . . . . . . . .  24
       3.2.5.  SendChat  . . . . . . . . . . . . . . . . . . . . . .  24
       3.2.6.  OnlineList  . . . . . . . . . . . . . . . . . . . . .  25
       3.2.7.  Disconnect  . . . . . . . . . . . . . . . . . . . . .  25
       3.2.8.  KickUser  . . . . . . . . . . . . . . . . . . . . . .  26
       3.2.9.  MuteUser  . . . . . . . . . . . . . . . . . . . . . .  26
       3.2.10. UnmuteUser  . . . . . . . . . . . . . . . . . . . . .  27
       3.2.11. SetAsAdmin  . . . . . . . . . . . . . . . . . . . . .  28
   4.  Peer-Peer Model . . . . . . . . . . . . . . . . . . . . . . .  29
     4.1.  Discovery . . . . . . . . . . . . . . . . . . . . . . . .  29
     4.2.  Handshake . . . . . . . . . . . . . . . . . . . . . . . .  31
     4.3.  Disconnect  . . . . . . . . . . . . . . . . . . . . . . .  33
   5.  Security Considerations . . . . . . . . . . . . . . . . . . .  33
     5.1.  Introduction  . . . . . . . . . . . . . . . . . . . . . .  33
     5.2.  Authentication  . . . . . . . . . . . . . . . . . . . . .  33
     5.3.  Encryption  . . . . . . . . . . . . . . . . . . . . . . .  33
     5.4.  Authenticity  . . . . . . . . . . . . . . . . . . . . . .  34
   6.  References  . . . . . . . . . . . . . . . . . . . . . . . . .  34
     6.1.  Normative References  . . . . . . . . . . . . . . . . . .  34
     6.2.  Informative References  . . . . . . . . . . . . . . . . .  34
   Author's Address  . . . . . . . . . . . . . . . . . . . . . . . .  34

1.  Introduction

   Chat servers are simple applications if written by a single entity.
   The lack of implementation rules adds complexity on ensuring that two
   applications communicate with each other properly.

2.  Message Formats

2.1.  Introduction

   Messages sent by both server-client and peer-peer models shall be
   encoded in JavaScript Object Notation (JSON).  This is to simplify
   the parsing process in the individual implementations.  There are
   several valid fields of the messages for this protocol.  Message
   formats are identified by the field "mtp".  The particular
   information for the rest of the fields will be discussed in detail in
   their respective use cases in the message formats.








Vargas                 Expires September 17, 2020               [Page 3]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.2.  Serverbound Messages

2.2.1.  Login

   This is the first packet a client should send to the server.  A
   sample message of this format is shown below:

   {"mtp": "Login", "data": {"name": "username"}}

   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string |   MessageType. All Login messages have this    |
   |         |        |              value set to "Login"              |
   |         |        |                                                |
   | name    | string |   Client Name. The username that the client    |
   |         |        |      requests to use to join the server.       |
   +---------+--------+------------------------------------------------+

                               Login Fields

   The server must send an AssignUsername message as a response to this
   message.  This is discussed in detail in Section 3.1.1

2.2.2.  SetUsername

   On the scenario that a client wishes to change usernames, this
   message will be sent to the server.  A sample message of this format
   is shown below:

   {"mtp": "SetUsername", "data": {"name": "new_name"}}

   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string |   MessageType. All SetUsername messages have   |
   |         |        |        this value set to "SetUsername"         |
   |         |        |                                                |
   | name    | string |       Requested new name by the client.        |
   +---------+--------+------------------------------------------------+

                            SetUsername Fields

   The server must send a SetUsername message as a response to this
   message.  This is discussed in detail in Section 3.2.1




Vargas                 Expires September 17, 2020               [Page 4]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.2.3.  RequestUserInfo

   On the scenario that a client wishes to view a user's network
   information, this message will be sent to the server.  A sample
   message of this format is shown below:

   {"mtp": "RequestUserInfo", "data": {"name": "username"}}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |  MessageType. All RequestUserInfo messages have |
   |        |        |       this value set to "RequestUserInfo"       |
   |        |        |                                                 |
   | name   | string |   Name of the user whose information is being   |
   |        |        |                    requested.                   |
   +--------+--------+-------------------------------------------------+

                          RequestUserInfo Fields

   The server must send a ProvideUserInfo message as a response to this
   message.  This is discussed in detail in Section 3.2.2

2.2.4.  RequestLocalTime

   On the scenario that a client wishes to change usernames, this
   message will be sent to the server.  A sample message of this format
   is shown below:

   {"mtp": "RequestLocalTime", "data": {}}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string | MessageType. All RequestLocalTime messages have |
   |        |        |       this value set to "RequestLocalTime"      |
   +--------+--------+-------------------------------------------------+

                          RequestLocalTime Fields

   The server must send a SendLocalTime message as a response to this
   message.  This is discussed in detail in Section 3.2.3







Vargas                 Expires September 17, 2020               [Page 5]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.2.5.  WhisperToUser

   On the scenario that a client wishes to send a private message to
   another user, this message will be sent to the server.  A sample
   message of this format is shown below:

   {"mtp": "WhisperToUser", "data": {"to": ["user1", "user2", "user3"],
   "message": "hi"}}

   +---------+------------+--------------------------------------------+
   | Field   | Data Type  |                Description                 |
   | Name    |            |                                            |
   +---------+------------+--------------------------------------------+
   | mtp     | string     |  MessageType. All WhisperToUser messages   |
   |         |            |   have this value set to "WhisperToUser"   |
   |         |            |                                            |
   | to      | array of   |    List of users the private message is    |
   |         | strings    |               addressed to.                |
   |         |            |                                            |
   | message | string     |      The private message to be sent.       |
   +---------+------------+--------------------------------------------+

                           WhisperToUser Fields

   Behavior is discussed in detail in Section 3.2.4

2.2.6.  SendChat

   On the scenario that a client wishes to send a message to everyone
   online, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "SendChat", "data": {"message": "hi"}}

   +---------+---------+-----------------------------------------------+
   | Field   | Data    |                  Description                  |
   | Name    | Type    |                                               |
   +---------+---------+-----------------------------------------------+
   | mtp     | string  |  MessageType. All SendChat messages have this |
   |         |         |            value set to "SendChat"            |
   |         |         |                                               |
   | message | string  |            The message to be sent.            |
   +---------+---------+-----------------------------------------------+

                              SendChat Fields

   Behavior is discussed in detail in Section 3.2.5




Vargas                 Expires September 17, 2020               [Page 6]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.2.7.  RequestOnlineList

   On the scenario that a client wishes to know all online users in the
   server, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "RequestOnlineList", "data": {}}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |   MessageType. All RequestOnlineList messages   |
   |        |        |    have this value set to "RequestOnlineList"   |
   +--------+--------+-------------------------------------------------+

                         RequestOnlineList Fields

   Behavior is discussed in detail in Section 3.2.6

2.2.8.  Disconnect

   On the scenario that a client wishes to disconnect, this message will
   be sent to the server.  A sample message of this format is shown
   below:

   {"mtp": "Disconnect", "data": {}}

   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string | MessageType. All Disconnect messages have this |
   |         |        |           value set to "Disconnect"            |
   +---------+--------+------------------------------------------------+

                             Disconnect Fields

   Behavior is discussed in detail in Section 3.2.7

2.2.9.  KickUser

   On the scenario that an admin wishes to remove a user from the
   server, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "KickUser", "data": {"name": "username"}}




Vargas                 Expires September 17, 2020               [Page 7]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +---------+---------+-----------------------------------------------+
   | Field   | Data    |                  Description                  |
   | Name    | Type    |                                               |
   +---------+---------+-----------------------------------------------+
   | mtp     | string  |  MessageType. All KickUser messages have this |
   |         |         |            value set to "KickUser"            |
   |         |         |                                               |
   | name    | string  |    Name of the user to be kicked out of the   |
   |         |         |                     server                    |
   +---------+---------+-----------------------------------------------+

                              KickUser Fields

   Behavior is discussed in detail in Section 3.2.8

2.2.10.  MuteUser

   On the scenario that an admin wishes to mute a user in the server,
   this message will be sent to the server.  A sample message of this
   format is shown below:

   {"mtp": "MuteUser", "data": {"name": "username"}}

   +---------+---------+-----------------------------------------------+
   | Field   | Data    |                  Description                  |
   | Name    | Type    |                                               |
   +---------+---------+-----------------------------------------------+
   | mtp     | string  |  MessageType. All MuteUser messages have this |
   |         |         |            value set to "MuteUser"            |
   |         |         |                                               |
   | name    | string  |   Name of the user to be muted in the server  |
   +---------+---------+-----------------------------------------------+

                              MuteUser Fields

   Behavior is discussed in detail in Section 3.2.9

2.2.11.  UnmuteUser

   On the scenario that an admin wishes to unmute a user in the server,
   this message will be sent to the server.  A sample message of this
   format is shown below:

   {"mtp": "UnmuteUser", "data": {"name": "username"}}







Vargas                 Expires September 17, 2020               [Page 8]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string | MessageType. All UnmuteUser messages have this |
   |         |        |           value set to "UnmuteUser"            |
   |         |        |                                                |
   | name    | string |  Name of the user to be unmuted in the server  |
   +---------+--------+------------------------------------------------+

                             UnmuteUser Fields

   Behavior is discussed in detail in Section 3.2.10

2.2.12.  SetAsAdmin

   On the scenario that an admin wishes to set another user as the
   admin, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "SetAsAdmin", "data": {"name": "username"}}

   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string | MessageType. All SetAsAdmin messages have this |
   |         |        |           value set to "SetAsAdmin"            |
   |         |        |                                                |
   | name    | string | Name of the user to be set as the new admin in |
   |         |        |                   the server                   |
   +---------+--------+------------------------------------------------+

                             SetAsAdmin Fields

   Behavior is discussed in detail in Section 3.2.11

2.3.  Clientbound Messages

2.3.1.  AssignUsername

   This is the message sent by the server to the client after receiving
   a Login message.

   If the requested username is valid:

   {"mtp": "AssignUsername", "data": {"name": "username"}, "status":
   "OK"}



Vargas                 Expires September 17, 2020               [Page 9]

Internet-Draft             CoE 151 MP Protocol                March 2020


   If the requested username has a duplicate:

   {"mtp": "AssignUsername", "data": {"name": "username"}, "status":
   "DuplicateError"}

   +--------+---------+------------------------------------------------+
   | Field  | Data    |                  Description                   |
   | Name   | Type    |                                                |
   +--------+---------+------------------------------------------------+
   | mtp    | string  | MessageType. All AssignUsername messages have  |
   |        |         |       this value set to "AssignUsername"       |
   |        |         |                                                |
   | name   | string  |         Accepted / Rejected username.          |
   |        |         |                                                |
   | status | Enum:   |    Possible values: "OK", "DuplicateError".    |
   |        | String  |  Status of the Login message previously sent.  |
   +--------+---------+------------------------------------------------+

                           AssignUsername Fields

2.3.2.  SetUsername

   This is the message sent by the server to the client after receiving
   a SetUsername message.

   If the requested username is valid:

   {"mtp": "SetUsername", "data": {"name": "username"}, "status": "OK"}

   If the requested username has a duplicate:

   {"mtp": "SetUsername", "data": {"name": "username"}, "status":
   "DuplicateError"}


















Vargas                 Expires September 17, 2020              [Page 10]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string | MessageType. All SetUsername messages have this |
   |        |        |            value set to "SetUsername"           |
   |        |        |                                                 |
   | name   | string |          Accepted / Rejected username.          |
   |        |        |                                                 |
   | status | Enum:  | Possible values: "OK", "DuplicateError". Status |
   |        | String |  of the SetUsername message previously sent by  |
   |        |        |                   the client.                   |
   +--------+--------+-------------------------------------------------+

                            SetUsername Fields

2.3.3.  ProvideUserInfo

   This is the message sent by the server to the client after receiving
   a RequestUserInfo message.

   If the requested user is online the server:

   {"mtp": "ProvideUserInfo", "data": {"name": "username", "ip":
   "192.168.1.1", "port": 15151}, "status": "OK"}

   If the requested user does not exist:

   {"mtp": "ProvideUserInfo", "data": {"name": "username", "ip":
   "192.168.1.1", "port": 15151}, "status": "UserDoesNotExist"}





















Vargas                 Expires September 17, 2020              [Page 11]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +--------+---------+------------------------------------------------+
   | Field  | Data    |                  Description                   |
   | Name   | Type    |                                                |
   +--------+---------+------------------------------------------------+
   | mtp    | string  | MessageType. All ProvideUserInfo messages have |
   |        |         |      this value set to "ProvideUserInfo"       |
   |        |         |                                                |
   | name   | string  |          Name of the requested user.           |
   |        |         |                                                |
   | ip     | string  |           IP of the requested user.            |
   |        |         |                                                |
   | port   | integer |          Port of the requested user.           |
   |        |         |                                                |
   | status | Enum:   |   Possible values: "OK", "UserDoesNotExist".   |
   |        | String  |     Status of the RequestUserInfo message      |
   |        |         |         previously sent by the client.         |
   +--------+---------+------------------------------------------------+

                          ProvideUserInfo Fields

2.3.4.  SendLocalTime

   This is the message sent by the server to the client after receiving
   a RequestLocalTime message.

   {"mtp": "SendLocalTime", "data": {"time": "08:30:55"}}

   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string |  MessageType. All SendLocalTime messages have  |
   |         |        |       this value set to "SendLocalTime"        |
   |         |        |                                                |
   | time    | string |            ISO 8601 formatted time.            |
   +---------+--------+------------------------------------------------+

                           SendLocalTime Fields

2.3.5.  WhisperFromUser

   This is the message sent by the server to the targetted client after
   receiving a WhisperToUser message.  A private message sent to a user.

   {"mtp": "WhisperFromUser", "data": {"from": "username", "message":
   "hi"}}





Vargas                 Expires September 17, 2020              [Page 12]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string | MessageType. All WhisperFromUser messages have |
   |         |        |      this value set to "WhisperFromUser"       |
   |         |        |                                                |
   | from    | string |     Name of the user who sent the message.     |
   |         |        |                                                |
   | message | string |            Private message content.            |
   +---------+--------+------------------------------------------------+

                          WhisperFromUser Fields

2.3.6.  SendChatFromUser

   This is the message sent by the server to all clients except the
   source after receiving a SendChat message.

   {"mtp": "SendChatFromUser", "data": {"name": "username", "message":
   "hi"}}

   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string |   MessageType. All SendChatFromUser messages   |
   |         |        |   have this value set to "SendChatFromUser"    |
   |         |        |                                                |
   | name    | string |     Name of the user who sent the message.     |
   |         |        |                                                |
   | message | string |                Message content.                |
   +---------+--------+------------------------------------------------+

                          SendChatFromUser Fields

2.3.7.  SendOnlineList

   This is the message sent by the server after receiving a
   RequestOnlineList message.

   {"mtp": "SendOnlineList", "data": {"names": ["user1", "user2",
   "user3"]}}








Vargas                 Expires September 17, 2020              [Page 13]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +--------+-----------+----------------------------------------------+
   | Field  | Data Type |                 Description                  |
   | Name   |           |                                              |
   +--------+-----------+----------------------------------------------+
   | mtp    | string    |   MessageType. All SendOnlineList messages   |
   |        |           |   have this value set to "SendOnlineList"    |
   |        |           |                                              |
   | names  | array of  |   Name of the users who are online in the    |
   |        | string    |                   server.                    |
   +--------+-----------+----------------------------------------------+

                           SendOnlineList Fields

2.3.8.  KickUser

   This is the message sent by the server after receiving a KickUser
   message from the client.

   If a kick is successful:

   {"mtp": "KickUser", "data": {}, status="OK"}

   If the requesting client is not an admin:

   {"mtp": "KickUser", "data": {}, status="NotAdminError"}

   If the requested user is not online in the server:

   {"mtp": "KickUser", "data": {}, status="UserDoesNotExist"}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |   MessageType. All KickUser messages have this  |
   |        |        |             value set to "KickUser"             |
   |        |        |                                                 |
   | status | Enum:  |    Possible values: "OK", "UserDoesNotExist",   |
   |        | String | "NotAdminError". Status of the KickUser message |
   |        |        |          previously sent by the client.         |
   +--------+--------+-------------------------------------------------+

                              KickUser Fields








Vargas                 Expires September 17, 2020              [Page 14]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.3.9.  MuteUser

   This is the message sent by the server after receiving a MuteUser
   message from the client.

   If a mute is successful:

   {"mtp": "MuteUser", "data": {}, status="OK"}

   If the requesting client is not an admin:

   {"mtp": "MuteUser", "data": {}, status="NotAdminError"}

   If the requested user is not online in the server:

   {"mtp": "MuteUser", "data": {}, status="UserDoesNotExist"}

   If the requested user is already muted:

   {"mtp": "MuteUser", "data": {}, status="UserAlreadyMuted"}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |   MessageType. All MuteUser messages have this  |
   |        |        |             value set to "MuteUser"             |
   |        |        |                                                 |
   | status | Enum:  |    Possible values: "OK", "UserDoesNotExist",   |
   |        | String | "NotAdminError", "UserAlreadyMuted".  Status of |
   |        |        |   the MuteUser message previously sent by the   |
   |        |        |                     client.                     |
   +--------+--------+-------------------------------------------------+

                              MuteUser Fields

2.3.10.  UnmuteUser

   This is the message sent by the server after receiving a UnmuteUser
   message from the client.

   If an unmute is successful:

   {"mtp": "UnmuteUser", "data": {}, status="OK"}

   If the requesting client is not an admin:

   {"mtp": "UnmuteUser", "data": {}, status="NotAdminError"}



Vargas                 Expires September 17, 2020              [Page 15]

Internet-Draft             CoE 151 MP Protocol                March 2020


   If the requested user is not online in the server:

   {"mtp": "UnmuteUser", "data": {}, status="UserDoesNotExist"}

   If the requested user is already unmuted:

   {"mtp": "UnmuteUser", "data": {}, status="UserAlreadyUnmuted"}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |  MessageType. All UnmuteUser messages have this |
   |        |        |            value set to "UnmuteUser"            |
   |        |        |                                                 |
   | status | Enum:  |    Possible values: "OK", "UserDoesNotExist",   |
   |        | String |  "NotAdminError", "UserAlreadyUnmuted".  Status |
   |        |        |   of the UnmuteUser message previously sent by  |
   |        |        |                   the client.                   |
   +--------+--------+-------------------------------------------------+

                             UnmuteUser Fields

2.3.11.  SetAsAdmin

   This is the message sent by the server after receiving a SetAsAdmin
   message from the client.

   If an admin privilege transfer is successful:

   {"mtp": "SetAsAdmin", "data": {}, status="OK"}

   If the requesting client is not an admin:

   {"mtp": "SetAsAdmin", "data": {}, status="NotAdminError"}

   If the requested user is not online in the server:

   {"mtp": "SetAsAdmin", "data": {}, status="UserDoesNotExist"}

   If the requested user is already an admin, (i.e. client set self as
   an admin):

   {"mtp": "SetAsAdmin", "data": {}, status="AlreadyAdmin"}







Vargas                 Expires September 17, 2020              [Page 16]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |  MessageType. All SetAsAdmin messages have this |
   |        |        |            value set to "SetAsAdmin"            |
   |        |        |                                                 |
   | status | Enum:  |    Possible values: "OK", "UserDoesNotExist",   |
   |        | String | "NotAdminError", "AlreadyAdmin".  Status of the |
   |        |        | KickUser message previously sent by the client. |
   +--------+--------+-------------------------------------------------+

                             SetAsAdmin Fields

2.3.12.  ServerMessage

   This is the message sent by the server to the client for general
   purposes (e.g. announcements).

   {"mtp": "ServerMessage", "data": {"message": "user has been kicked
   from the server."}}

   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string |  MessageType. All ServerMessage messages have  |
   |         |        |       this value set to "ServerMessage"        |
   |         |        |                                                |
   | message | string |                Message content                 |
   +---------+--------+------------------------------------------------+

                           ServerMessage Fields

2.3.13.  Disconnect

   This is the message sent by the server on terminating the connection.
   The reason for disconnecting is also included in the message.

   {"mtp": "Disconnect", "data": {"message": "Server closing."}}











Vargas                 Expires September 17, 2020              [Page 17]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string | MessageType. All Disconnect messages have this |
   |         |        |           value set to "Disconnect"            |
   |         |        |                                                |
   | message | string |           Reason for disconnecting.            |
   +---------+--------+------------------------------------------------+

                             Disconnect Fields

2.4.  Peer-Peer Messages

2.4.1.  Discovery

   Type: Broadcasted

   Discovery message is sent to the broadcast network to ping the list
   of active peers.  This queries the hostnames and usernames of these
   peers.  This is not completely necessary to implement if you will add
   a feature that will connect the peer to another by manually inputting
   the ip and port of the peer.  However, it is strongly recommended you
   implement this message to ensure smooth testing.

   {"mtp": "Discovery", "data": {"ip": "192.168.1.1", "port": 15151}}

   +---------+---------+-----------------------------------------------+
   | Field   | Data    |                  Description                  |
   | Name    | Type    |                                               |
   +---------+---------+-----------------------------------------------+
   | mtp     | string  | MessageType. All Discovery messages have this |
   |         |         |            value set to "Discovery"           |
   |         |         |                                               |
   | ip      | string  |            IP of the querying peer.           |
   |         |         |                                               |
   | port    | integer |           Port of the querying peer.          |
   +---------+---------+-----------------------------------------------+

                             Discovery Fields

2.4.2.  DiscoveryResponse

   Type: Direct / Single

   DiscoveryResponse is sent by peers who are active in the network.





Vargas                 Expires September 17, 2020              [Page 18]

Internet-Draft             CoE 151 MP Protocol                March 2020


   {"mtp": "DiscoveryResponse", "data": {"ip": "192.168.1.1", "port":
   15151,"name": "username"}}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |   MessageType. All DiscoveryResponse messages   |
   |        |        |    have this value set to "DiscoveryResponse"   |
   |        |        |                                                 |
   | name   | string |         Username of the responding peer.        |
   |        |        |                                                 |
   | ip     | string |            IP of the responding peer.           |
   |        |        |                                                 |
   | port   | string |           Port of the responding peer.          |
   +--------+--------+-------------------------------------------------+

                         DiscoveryResponse Fields

2.4.3.  Handshake

   Type: Broadcasted

   The handshake message is required to be sent for each scenario
   wherein a new peer-peer joins a network.  There may be peers active
   in the network or not but this message type attempts to ensure that
   the username to be used by the entering peer is not a duplicate of
   another user existing in the network.

   {"mtp": "Handshake", "data": {"ip": "192.168.0.1", "port": 15151,
   "name": "new_peer"}}

   +--------+---------+------------------------------------------------+
   | Field  | Data    |                  Description                   |
   | Name   | Type    |                                                |
   +--------+---------+------------------------------------------------+
   | mtp    | string  | MessageType. All Handshake messages have this  |
   |        |         |            value set to "Handshake"            |
   |        |         |                                                |
   | ip     | string  |   IP of the peer broadcasting the Handshake    |
   |        |         |                                                |
   | port   | integer |  Port of the peer broadcasting the Handshake.  |
   |        |         |                                                |
   | name   | string  | Username that is being broadcasted to all the  |
   |        |         |   other peers in the network for validation.   |
   +--------+---------+------------------------------------------------+

                             Handshake Fields



Vargas                 Expires September 17, 2020              [Page 19]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.4.4.  HandshakeResponse

   Type: Direct / Single

   The Handshake Response is sent back by peers active in the network to
   the ip and port found in the Handshake message sent by new peers
   attempting to join the network.  This message type can only be sent
   if the peer is considered as active in the network and the peer has a
   valid username.

   If the username contained in the Handshake is not equal to the
   username held by the responding peer: {"mtp": "HandshakeResponse",
   "data": {"name": "username"}, "status": "OK"}

   If the username contained in the Handshake is equal to the username
   held by the responding peer: {"mtp": "HandshakeResponse", "data":
   {"name": "username"}, "status": "DuplicateError"}

   +--------+--------+-------------------------------------------------+
   | Field  | Data   |                   Description                   |
   | Name   | Type   |                                                 |
   +--------+--------+-------------------------------------------------+
   | mtp    | string |   MessageType. All HandshakeResponse messages   |
   |        |        |    have this value set to "HandshakeResponse"   |
   |        |        |                                                 |
   | name   | string |           Accepted/Rejected username.           |
   |        |        |                                                 |
   | status | Enum:  |     Possible values: "OK", "DuplicateError".    |
   |        | String |    Validation status of the Handshake message   |
   |        |        |     received prior to sending this message.     |
   +--------+--------+-------------------------------------------------+

                         HandshakeResponse Fields

2.4.5.  SendChat

   Type: Broadcasted

   SendChat is a message format for general chat messages.

   {"mtp": "SendChat", "data": {"message": "hi"}}










Vargas                 Expires September 17, 2020              [Page 20]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +---------+---------+-----------------------------------------------+
   | Field   | Data    |                  Description                  |
   | Name    | Type    |                                               |
   +---------+---------+-----------------------------------------------+
   | mtp     | string  |  MessageType. All SendChat messages have this |
   |         |         |            value set to "SendChat"            |
   |         |         |                                               |
   | message | string  |                Message content.               |
   +---------+---------+-----------------------------------------------+

                              SendChat Fields

2.4.6.  Whisper

   Type: Direct / Single

   Whisper is a message format for private chat messages.

   {"mtp": "Whisper", "data": {"message": "hi"}}

   +---------+---------+-----------------------------------------------+
   | Field   | Data    |                  Description                  |
   | Name    | Type    |                                               |
   +---------+---------+-----------------------------------------------+
   | mtp     | string  |  MessageType. All Whisper messages have this  |
   |         |         |             value set to "Whisper"            |
   |         |         |                                               |
   | message | string  |            Private message content.           |
   +---------+---------+-----------------------------------------------+

                              Whisper Fields

2.4.7.  Disconnect

   Type: Broadcasted

   Peers send a Disconnect message to inform other peers on leaving the
   network.

   {"mtp": "Disconnect", "data": {}}











Vargas                 Expires September 17, 2020              [Page 21]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +---------+--------+------------------------------------------------+
   | Field   | Data   |                  Description                   |
   | Name    | Type   |                                                |
   +---------+--------+------------------------------------------------+
   | mtp     | string | MessageType. All Disconnect messages have this |
   |         |        |           value set to "Disconnect"            |
   +---------+--------+------------------------------------------------+

                             Disconnect Fields

3.  Server-Client Model

3.1.  States / Phases

3.1.1.  Login

   The Login States include sending username from client to server and
   sending "OK" status from server to client to ensure both are
   connected.

       +----------+                             +----------+
       |  Client  |>---------| Login |--------->|  Server  |
       +----------+      username: "Daren"      +----------+

       +----------+                             +----------+
       |  Client  |<<---| AssignUsername |----<<|  Server  |
       +----------+         status: "OK"        +----------+

       +----------+                             +----------+
       |  Client  |<<------------------------->>|  Server  |
       +----------+          Connected          +----------+

                                 Figure 1

3.2.  Request-Response Behavior

3.2.1.  SetUsername

   The SetUsername States include sending new username from client to
   server and sending "OK" or "DuplicateError" status from server to
   client to set client's new username.










Vargas                 Expires September 17, 2020              [Page 22]

Internet-Draft             CoE 151 MP Protocol                March 2020


       +----------+                             +----------+
       |  Client  |>------| SetUsername |------>|  Server  |
       +----------+      SetUsername: "Daren"   +----------+

       +----------+                             +----------+
       |  Client  |<<-----| SetUsername |-----<<|  Server  |
       +----------+         status: "OK"        +----------+

       +----------+                             +----------+
       |  Client  |<<-----| SetUsername |-----<<|  Server  |
       +----------+     status: "DuplicateError"+----------+

                                 Figure 2

3.2.2.  UserInfo

   The UserInfo States include sending username from client to server
   and sending "OK" or "UserDoesNotExist" status from server to client
   along with user information if it exists.

       +----------+                             +----------+
       |  Client  |>----| RequestUserInfo |---->|  Server  |
       +----------+      SetUsername: "Daren"   +----------+

       +----------+                             +----------+
       |  Client  |<<---| ProvideUserInfo |---<<|  Server  |
       +----------+         status: "OK"        +----------+

        +----------+                             +----------+
       |  Client  |<<---| ProvideUserInfo |---<<|  Server  |
       +----------+   status: "UserDoesNotExist" +----------+


                                 Figure 3

3.2.3.  LocalTime

   The LocalTime States include a request from client to server and the
   local time response from server to client.












Vargas                 Expires September 17, 2020              [Page 23]

Internet-Draft             CoE 151 MP Protocol                March 2020


       +----------+                             +----------+
       |  Client  |>----| RequestLocalTime |--->|  Server  |
       +----------+                             +----------+

       +----------+                             +----------+
       |  Client  |<<---| SendLocalTime |---<<|  Server  |
       +----------+       time: "00:00:00"      +----------+




                                 Figure 4

3.2.4.  Whisper

   Description here

   +----------+                           +----------+     +----------+
   |  Client  |>----| WhisperToUser |---->|  Server  |     | Client_B |
   +----------+        To: "Client_B"     +----------+     +----------+

   +----------+                           +----------+ msg +----------+
   |  Client  |                           |  Server  |---->| Client_B |
   +----------+                           +----------+     +----------+

   +----------+                           +----------+     +----------+
   |  Client  |<<---| ServerMessage |---<<|  Server  |     | Client_B |
   +----------+ message:"UserDoesNotExist"+----------+     +----------+




                                 Figure 5

3.2.5.  SendChat

   Description here














Vargas                 Expires September 17, 2020              [Page 24]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +----------+          +---------+
   |          | SendChat |         |
   |  Client1 +--------->+ Server  |
   |          |   msg    |         |
   +----------+          +----+----+
                              |
                              |
                +----------------------------+
                |             |              |
                |             |              |
                |msg          |msg           |msg
                |             v              v
          +-----v-----+ +-----+-----+  +-----+-----+
          |           | |           |  |           |
          |  Client2  | |  Client3  |  |  Client4  |
          |           | |           |  |           |
          +-----------+ +-----------+  +-----------+



                                 Figure 6

3.2.6.  OnlineList

   Description here

       +----------+                             +----------+
       |  Client  |>---| RequestOnlineList |--->|  Server  |
       +----------+                             +----------+

       +----------+                            +----------+
       |  Client  |<<---| SendOnlineList |---<<|  Server  |
       +----------+                            +----------+




                                 Figure 7

3.2.7.  Disconnect

   Description here









Vargas                 Expires September 17, 2020              [Page 25]

Internet-Draft             CoE 151 MP Protocol                March 2020


       +----------+                             +----------+
       |  Client  |>-------| Disconnect |------>|  Server  |
       +----------+                             +----------+

                                                +----------+
                                                |  Server  |
                                                +----------+




                                 Figure 8

3.2.8.  KickUser

   Description here

       +----------+                             +----------+
       |  Client  |>--------| KickUser |------->|  Server  |
       +----------+      name : "Client2"       +----------+

       +----------+                            +----------+
       |  Client  |<<------| KickUser |------<<|  Server  |
       +----------+      status : "OK"         +----------+

       +----------+                            +----------+
       |  Client  |<<------| KickUser |------<<|  Server  |
       +----------+  status : "NotAdminError"  +----------+

       +----------+                            +----------+
       |  Client  |<<------| KickUser |------<<|  Server  |
       +----------+ status : "UserDoesNotExist"+----------+




                                 Figure 9

3.2.9.  MuteUser

   Description here










Vargas                 Expires September 17, 2020              [Page 26]

Internet-Draft             CoE 151 MP Protocol                March 2020


       +----------+                             +----------+
       |  Client  |>--------| MuteUser |------->|  Server  |
       +----------+      name : "Client2"       +----------+

       +----------+                            +----------+
       |  Client  |<<------| MuteUser |------<<|  Server  |
       +----------+      status : "OK"         +----------+

       +----------+                            +----------+
       |  Client  |<<------| MuteUser |------<<|  Server  |
       +----------+  status : "NotAdminError"  +----------+

       +----------+                            +----------+
       |  Client  |<<------| MuteUser |------<<|  Server  |
       +----------+ status : "UserDoesNotExist"+----------+

       +----------+                            +----------+
       |  Client  |<<------| MuteUser |------<<|  Server  |
       +----------+ status : "UserAlreadyMuted"+----------+


                                 Figure 10

3.2.10.  UnmuteUser

   Description here

























Vargas                 Expires September 17, 2020              [Page 27]

Internet-Draft             CoE 151 MP Protocol                March 2020


       +----------+                               +----------+
       |  Client  |>--------| UnmuteUser |------->|  Server  |
       +----------+      name : "Client2"         +----------+

       +----------+                              +----------+
       |  Client  |<<------| UnmuteUser |------<<|  Server  |
       +----------+      status : "OK"           +----------+

       +----------+                              +----------+
       |  Client  |<<------| UnmuteUser |------<<|  Server  |
       +----------+  status : "NotAdminError"    +----------+

       +----------+                              +----------+
       |  Client  |<<------| UnmuteUser |------<<|  Server  |
       +----------+ status : "UserDoesNotExist"  +----------+

       +----------+                              +----------+
       |  Client  |<<------| UnmuteUser |------<<|  Server  |
       +----------+ status : "UserNotMuted"      +----------+


                                 Figure 11

3.2.11.  SetAsAdmin

   Description here

























Vargas                 Expires September 17, 2020              [Page 28]

Internet-Draft             CoE 151 MP Protocol                March 2020


       +----------+                               +----------+
       |  Client  |>--------| SetAsAdmin |------->|  Server  |
       +----------+      name : "Client2"         +----------+

       +----------+                              +----------+
       |  Client  |<<------| SetAsAdmin |------<<|  Server  |
       +----------+      status : "OK"           +----------+

       +----------+                              +----------+
       |  Client  |<<------| SetAsAdmin |------<<|  Server  |
       +----------+  status : "NotAdminError"    +----------+

       +----------+                              +----------+
       |  Client  |<<------| SetAsAdmin |------<<|  Server  |
       +----------+ status : "UserDoesNotExist"  +----------+

       +----------+                              +----------+
       |  Client  |<<------| SetAsAdmin|------<<|  Server  |
       +----------+ status : "AlreadyAdmin"      +----------+


                                 Figure 12

4.  Peer-Peer Model

4.1.  Discovery

   Description here























Vargas                 Expires September 17, 2020              [Page 29]

Internet-Draft             CoE 151 MP Protocol                March 2020


                         +---------+
                         |         |
                         | NewPeer |
                         |         |
                         +----+----+
                              |
                              |
                +----------------------------+
                |             |              |
                |             |              |
                |Discovery    |Discovery     |Discovery
                |             |              |
          +-----v-----+ +-----v-----+  +-----v-----+
          |           | |           |  |           |
          |   Peer1   | |   Peer2   |  |   Peer3   |
          |           | |           |  |           |
          +-----------+ +-----------+  +-----------+


                                 Figure 13

   Description here


                         +---------+
                         |         |
                |------->| NewPeer |<--------|
                |        |         |         |
                |        +----+----+         |
                |             ^              |
                |             |              |
                |             |              |
                |             |              |
                |Discovery    |Discovery     |Discovery
                |Response     |Response      |Response
                |             |              |
          +-----|-----+ +-----|-----+  +-----|-----+
          |           | |           |  |           |
          |   Peer1   | |   Peer2   |  |   Peer3   |
          |           | |           |  |           |
          +-----------+ +-----------+  +-----------+


                                 Figure 14







Vargas                 Expires September 17, 2020              [Page 30]

Internet-Draft             CoE 151 MP Protocol                March 2020


4.2.  Handshake

   Description here


                         +---------+
                         |         |
                         |  Peer4  |
                         |         |
                         +----+----+
                              |
                              |
                +----------------------------+
                |             |              |
                |             |              |
                |Handshake    |Handshake     |Handshake
                |             |              |
          +-----v-----+ +-----v-----+  +-----v-----+
          |           | |           |  |           |
          |   Peer1   | |   Peer2   |  |   Peer3   |
          |           | |           |  |           |
          +-----------+ +-----------+  +-----------+


                                 Figure 15

   Description here
























Vargas                 Expires September 17, 2020              [Page 31]

Internet-Draft             CoE 151 MP Protocol                March 2020


                         +---------+
                         |         |
                |------->|  Peer4  |<--------|
                |        |         |         |
                |        +----+----+         |
                |             ^              |
                |             |              |
                |             |              |
                |Handshake    |Handshake     |Handshake
                |Response     |Response      |Response
                |status: "OK" |status: "OK"  |status: "OK"
                |             |              |
          +-----|-----+ +-----|-----+  +-----|-----+
          |           | |           |  |           |
          |   Peer1   | |   Peer2   |  |   Peer3   |
          |           | |           |  |           |
          +-----------+ +-----------+  +-----------+


                                 Figure 16

   Description here


                           +----------+
                           |          |
                |--------->|  Peer1   |<-----------|
                |          |          |            |
                |          +----------+            |
                |                ^                 |
                |                |                 |
                |                |                 |
           Handshake         Handshake          Handshake
           Response          Response           Response
           status:           status:            status:
        "DuplicateError"  "DuplicateError"   "DuplicateError"
                |                |                 |
          +-----|-----+    +-----|-----+     +-----|-----+
          |           |    |           |     |           |
          |   Peer1   |    |   Peer2   |     |   Peer3   |
          |           |    |           |     |           |
          +-----------+    +-----------+     +-----------+


                                 Figure 17






Vargas                 Expires September 17, 2020              [Page 32]

Internet-Draft             CoE 151 MP Protocol                March 2020


4.3.  Disconnect

   Description here


                         +---------+
                         |         |
                         |  Peer4  |
                         |         |
                         +----+----+
                              |
                              |
                +----------------------------+
                |             |              |
                |             |              |
                |Disconnect   |Disconnect    |Disconnect
                |             |              |
          +-----v-----+ +-----v-----+  +-----v-----+
          |           | |           |  |           |
          |   Peer1   | |   Peer2   |  |   Peer3   |
          |           | |           |  |           |
          +-----------+ +-----------+  +-----------+


                                 Figure 18

5.  Security Considerations

5.1.  Introduction

   This protocol will raise plenty of security vulnerabilities.  This is
   expected as security was never considered in designing this protocol.
   This section will simply discuss some of possible vulnerabilities.

5.2.  Authentication

   There is no form of authentication in this protocol.  The chat system
   is not anonymous yet anyone can choose a name.

5.3.  Encryption

   Private messages are easily sniffed by a packet sniffer like
   Wireshark.  The private messages are sent in plaintext and are
   therefore not secure.







Vargas                 Expires September 17, 2020              [Page 33]

Internet-Draft             CoE 151 MP Protocol                March 2020


5.4.  Authenticity

   There is no method to ensure that the data has not been tampered
   with.  There is no form of encrypted checksum encoded in the message.

6.  References

6.1.  Normative References

   [RFC3470]  Hollenbeck, S., Rose, M., and L. Masinter, "Guidelines for
              the Use of Extensible Markup Language (XML) within IETF
              Protocols", RFC 3470, May 2013.

              This is a primary reference work.

6.2.  Informative References

   [RFC6949]  Flanagan, H. and N. Brownlee, "RFC Series Format
              Requirements and Future Development", RFC 6949, May 2013.

              This is a primary reference work.

Author's Address

   Keith Vargas
   Electrical and Electronics Engineering Institute
   P. Velasquez St.
   Quezon City, Metro Manila  1101
   Philippines

   Email: keith.vargas@eee.upd.edu.ph




















Vargas                 Expires September 17, 2020              [Page 34]
