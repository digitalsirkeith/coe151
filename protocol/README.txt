



CoE 151                                                        K. Vargas
Internet-Draft                                                      EEEI
Updates: 5 (if approved)                                  March 14, 2020
Intended status: Standards Track
Expires: September 15, 2020


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

   This Internet-Draft will expire on September 15, 2020.

Copyright Notice

   Copyright (c) 2020 IETF Trust and the persons identified as the
   document authors.  All rights reserved.

   This document is subject to BCP 78 and the IETF Trust's Legal
   Provisions Relating to IETF Documents



Vargas                 Expires September 15, 2020               [Page 1]

Internet-Draft             CoE 151 MP Protocol                March 2020


   (https://trustee.ietf.org/license-info) in effect on the date of
   publication of this document.  Please review these documents
   carefully, as they describe your rights and restrictions with respect
   to this document.

1.  Introduction

   Chat servers are simple applications if written by a single entity.
   The lack of implementation rules adds complexity on ensuring that two
   applications communicate with each other properly.

2.  Message Formats

2.1.  Introduction

   Messages sent by both server-client and peer-peer models shall be
   encoded in JavaScript Object Notation (JSON).  This is to simplify
   the parsing process in the individual implementations.  There are
   several valid entries of the messages for this protocol.  Message
   formats are identified by the entry "mtp".  The particular
   information for the rest of the entries will be discussed in detail
   in their respective use cases in the message formats.

2.2.  Serverbound Messages

2.2.1.  Login

   This is the first packet a client should send to the server.  A
   sample message of this format is shown below:

   {"mtp": "Login", "data": {"name": "username"}}

   +-------+--------+--------------------------------------------------+
   | Entry | Data   |                   Description                    |
   |       | Type   |                                                  |
   +-------+--------+--------------------------------------------------+
   | mtp   | string | MessageType. All Login messages have this value  |
   |       |        |                  set to "Login"                  |
   |       |        |                                                  |
   | name  | string |    Client Name. The username that the client     |
   |       |        |       requests to use to join the server.        |
   +-------+--------+--------------------------------------------------+

                               Login Entries

   The server must send an AssignUsername message as a response to this
   message.  This is discussed in detail in Section 3.1.1




Vargas                 Expires September 15, 2020               [Page 2]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.2.2.  SetUsername

   On the scenario that a client wishes to change usernames, this
   message will be sent to the server.  A sample message of this format
   is shown below:

   {"mtp": "SetUsername", "data": {"name": "new_name"}}

   +-------+--------+--------------------------------------------------+
   | Entry | Data   |                   Description                    |
   |       | Type   |                                                  |
   +-------+--------+--------------------------------------------------+
   | mtp   | string | MessageType. All SetUsername messages have this  |
   |       |        |            value set to "SetUsername"            |
   |       |        |                                                  |
   | name  | string |        Requested new name by the client.         |
   +-------+--------+--------------------------------------------------+

                            SetUsername Entries

   The server must send a SetUsername message as a response to this
   message.  This is discussed in detail in Section 3.2.1

2.2.3.  RequestUserInfo

   On the scenario that a client wishes to view a user's network
   information, this message will be sent to the server.  A sample
   message of this format is shown below:

   {"mtp": "RequestUserInfo", "data": {"name": "username"}}

   +-------+--------+--------------------------------------------------+
   | Entry | Data   |                   Description                    |
   |       | Type   |                                                  |
   +-------+--------+--------------------------------------------------+
   | mtp   | string |  MessageType. All RequestUserInfo messages have  |
   |       |        |       this value set to "RequestUserInfo"        |
   |       |        |                                                  |
   | name  | string |   Name of the user whose information is being    |
   |       |        |                    requested.                    |
   +-------+--------+--------------------------------------------------+

                          RequestUserInfo Entries

   The server must send a ProvideUserInfo message as a response to this
   message.  This is discussed in detail in Section 3.2.2





Vargas                 Expires September 15, 2020               [Page 3]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.2.4.  RequestLocalTime

   On the scenario that a client wishes to change usernames, this
   message will be sent to the server.  A sample message of this format
   is shown below:

   {"mtp": "RequestLocalTime", "data": {}}

   +-------+--------+--------------------------------------------------+
   | Entry | Data   |                   Description                    |
   |       | Type   |                                                  |
   +-------+--------+--------------------------------------------------+
   | mtp   | string | MessageType. All RequestLocalTime messages have  |
   |       |        |       this value set to "RequestLocalTime"       |
   +-------+--------+--------------------------------------------------+

                         RequestLocalTime Entries

   The server must send a SendLocalTime message as a response to this
   message.  This is discussed in detail in Section 3.2.3

2.2.5.  WhisperToUser

   On the scenario that a client wishes to send a private message to
   another user, this message will be sent to the server.  A sample
   message of this format is shown below:

   {"mtp": "WhisperToUser", "data": {"to": ["user1", "user2", "user3"],
   "message": "hi"}}

   +---------+-----------+---------------------------------------------+
   | Entry   | Data Type |                 Description                 |
   +---------+-----------+---------------------------------------------+
   | mtp     | string    |   MessageType. All WhisperToUser messages   |
   |         |           |    have this value set to "WhisperToUser"   |
   |         |           |                                             |
   | to      | array of  |     List of users the private message is    |
   |         | strings   |                addressed to.                |
   |         |           |                                             |
   | message | string    |       The private message to be sent.       |
   +---------+-----------+---------------------------------------------+

                           WhisperToUser Entries

   Behavior is discussed in detail in Section 3.2.4






Vargas                 Expires September 15, 2020               [Page 4]

Internet-Draft             CoE 151 MP Protocol                March 2020


2.2.6.  SendChat

   On the scenario that a client wishes to send a message to everyone
   online, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "SendChat", "data": {"message": "hi"}}

   +---------+---------+-----------------------------------------------+
   | Entry   | Data    |                  Description                  |
   |         | Type    |                                               |
   +---------+---------+-----------------------------------------------+
   | mtp     | string  |  MessageType. All SendChat messages have this |
   |         |         |            value set to "SendChat"            |
   |         |         |                                               |
   | message | string  |            The message to be sent.            |
   +---------+---------+-----------------------------------------------+

                             SendChat Entries

   Behavior is discussed in detail in Section 3.2.5

2.2.7.  RequestOnlineList

   On the scenario that a client wishes to know all online users in the
   server, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "RequestOnlineList", "data": {}}

   +-------+--------+--------------------------------------------------+
   | Entry | Data   |                   Description                    |
   |       | Type   |                                                  |
   +-------+--------+--------------------------------------------------+
   | mtp   | string | MessageType. All RequestOnlineList messages have |
   |       |        |      this value set to "RequestOnlineList"       |
   +-------+--------+--------------------------------------------------+

                         RequestOnlineList Entries

   Behavior is discussed in detail in Section 3.2.6

2.2.8.  Disconnect

   On the scenario that a client wishes to disconnect, this message will
   be sent to the server.  A sample message of this format is shown
   below:




Vargas                 Expires September 15, 2020               [Page 5]

Internet-Draft             CoE 151 MP Protocol                March 2020


   {"mtp": "Disconnect", "data": {}}

   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |  MessageType. All Disconnect messages have this |
   |       |         |            value set to "Disconnect"            |
   +-------+---------+-------------------------------------------------+

                            Disconnect Entries

   Behavior is discussed in detail in Section 3.2.7

2.2.9.  KickUser

   On the scenario that an admin wishes to remove a user from the
   server, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "KickUser", "data": {"name": "username"}}

   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |   MessageType. All KickUser messages have this  |
   |       |         |             value set to "KickUser"             |
   |       |         |                                                 |
   | name  | string  | Name of the user to be kicked out of the server |
   +-------+---------+-------------------------------------------------+

                             KickUser Entries

   Behavior is discussed in detail in Section 3.2.8

2.2.10.  MuteUser

   On the scenario that an admin wishes to mute a user in the server,
   this message will be sent to the server.  A sample message of this
   format is shown below:

   {"mtp": "MuteUser", "data": {"name": "username"}}








Vargas                 Expires September 15, 2020               [Page 6]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |   MessageType. All MuteUser messages have this  |
   |       |         |             value set to "MuteUser"             |
   |       |         |                                                 |
   | name  | string  |    Name of the user to be muted in the server   |
   +-------+---------+-------------------------------------------------+

                             MuteUser Entries

   Behavior is discussed in detail in Section 3.2.9

2.2.11.  UnmuteUser

   On the scenario that an admin wishes to unmute a user in the server,
   this message will be sent to the server.  A sample message of this
   format is shown below:

   {"mtp": "UnmuteUser", "data": {"name": "username"}}

   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |  MessageType. All UnmuteUser messages have this |
   |       |         |            value set to "UnmuteUser"            |
   |       |         |                                                 |
   | name  | string  |   Name of the user to be unmuted in the server  |
   +-------+---------+-------------------------------------------------+

                            UnmuteUser Entries

   Behavior is discussed in detail in Section 3.2.10

2.2.12.  SetAsAdmin

   On the scenario that an admin wishes to set another user as the
   admin, this message will be sent to the server.  A sample message of
   this format is shown below:

   {"mtp": "SetAsAdmin", "data": {"name": "username"}}








Vargas                 Expires September 15, 2020               [Page 7]

Internet-Draft             CoE 151 MP Protocol                March 2020


   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |  MessageType. All SetAsAdmin messages have this |
   |       |         |            value set to "SetAsAdmin"            |
   |       |         |                                                 |
   | name  | string  |  Name of the user to be set as the new admin in |
   |       |         |                    the server                   |
   +-------+---------+-------------------------------------------------+

                            SetAsAdmin Entries

   Behavior is discussed in detail in Section 3.2.11

2.3.  Clientbound Messages

   message

2.4.  Peer-Peer Messages
	A different set of message types are used for the peer-peer model. However,
	these messages are still encoded in JavaScript Object Notation (JSON). 
	
2.4.1 Handshake

   The handshake message is required to be sent for each scenario wherein a new peer-peer
   joins a network. There may be peers active in the network or not but this message type 
   attempts to ensure that the username to be used by the entering peer is not a duplicate
   of another user existing in the network.
   
   {"mtp": "Handshake", "data": {"ip": "192.168.0.1", "port": 15151, "name": "new_peer"}}
   
   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |  MessageType. All Handshake messages have this  |
   |       |         |            value set to "Handshake"             |
   |       |         |                                                 |
   | ip    | string  |  IP of the peer broadcasting the Handshake      |
   |       |         |                                                 |
   |	   |         |                                                 |
   | port  |  int    |  port of the peer broadcasting the Handshake    |
   |	   |         |                                                 |
   |	   |         |                                                 |
   | name  | string  |  username that is being broadcasted to all      |
   |	   |         |    the other peers in the network               |
   |	   |         |             for validation.                     |
   +-------+---------+-------------------------------------------------+
  
2.4.2 Handshake Response

	The Handshake Response is sent back by peers active in the network to the ip and port 
	found in the Handshake message sent by new peers attempting to join the network. This 
	message type can only be sent if the peer is considered as active in the network and 
	the peer has a valid username.
	
	If the username contained in the Handshake is not equal to the username held by the 
	responding peer:
	{"mtp": "HandshakeResponse", "data": {}, "status": "OK"}
	
	If the username contained in the Handshake is equal to the username held by the 
	responding peer:
	{"mtp": "HandshakeResponse", "data": {}, "status": "DuplicateError"}
	
   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |  MessageType. All HandshakeResponse messages    |
   |       |         |    have this value set to "SetAsAdmin"          |
   |       |         |                                                 |
   |status | string  |  Validation status of the Handshake message     |
   |       |         |    received prior to sending this message       |
   +-------+---------+-------------------------------------------------+
   
3.  Server-Client Model

3.1.  States / Phases

3.1.1.  Login

   Placeholder

3.2.  Request-Response Behavior

3.2.1.  SetUsername

   Placeholder

3.2.2.  UserInfo

   Placeholder

3.2.3.  LocalTime

   Placeholder







Vargas                 Expires September 15, 2020               [Page 8]

Internet-Draft             CoE 151 MP Protocol                March 2020


3.2.4.  Whisper

   Placeholder

3.2.5.  SendChat

   Placeholder

3.2.6.  OnlineList

   Placeholder

3.2.7.  Disconnect

   Placeholder

3.2.8.  KickUser

   Placeholder

3.2.9.  MuteUser

   Placeholder

3.2.10.  UnmuteUser

   Placeholder

3.2.11.  SetAsAdmin

   Placeholder

4.  References

4.1.  Normative References

   [RFC3470]  Hollenbeck, S., Rose, M., and L. Masinter, "Guidelines for
              the Use of Extensible Markup Language (XML) within IETF
              Protocols", RFC 3470, May 2013.

              This is a primary reference work.

4.2.  Informative References

   [RFC6949]  Flanagan, H. and N. Brownlee, "RFC Series Format
              Requirements and Future Development", RFC 6949, May 2013.

              This is a primary reference work.



Vargas                 Expires September 15, 2020               [Page 9]

Internet-Draft             CoE 151 MP Protocol                March 2020


Author's Address

   Keith Vargas
   Electrical and Electronics Engineering Institute
   P. Velasquez St.
   Quezon City, Metro Manila  1101
   Philippines

   Email: keith.vargas@eee.upd.edu.ph










































Vargas                 Expires September 15, 2020              [Page 10]
