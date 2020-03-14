



CoE 151                                                        K. Vargas
Internet-Draft                                                      EEEI
Updates: 3 (if approved)                                  March 14, 2020
Intended status: Standards Track
Expires: September 15, 2020


           Server-Client and Peer-Peer Communication Protocol
                        coe-151-mp1-protocol-03

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
   | mtp   | string |  MessageType. All Login messages have the value  |
   |       |        |              of mtp set to "Login"               |
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
   message will be sent.  A sample message of this format is shown
   below:

   {"mtp": "SetUsername", "data": {"name": "new_name"}}

   +-------+---------+-------------------------------------------------+
   | Entry | Data    |                   Description                   |
   |       | Type    |                                                 |
   +-------+---------+-------------------------------------------------+
   | mtp   | string  |  MessageType. All Login messages have the value |
   |       |         |              of mtp set to "Login"              |
   |       |         |                                                 |
   | name  | string  |        Requested new name by the client.        |
   +-------+---------+-------------------------------------------------+

                            SetUsername Entries

   The server must send a SetUsername message as a response to this
   message.  This is discussed in detail in Section 3.2.1

2.3.  Clientbound Messages

   message

2.4.  Peer-Peer Messages

   message

3.  Server-Client Model

3.1.  States / Phases

3.1.1.  Login

   hi

3.2.  Request-Response Behavior

3.2.1.  SetUsername

   hi







Vargas                 Expires September 15, 2020               [Page 3]

Internet-Draft             CoE 151 MP Protocol                March 2020


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

Author's Address

   Keith Vargas
   Electrical and Electronics Engineering Institute
   P. Velasquez St.
   Quezon City, Metro Manila  1101
   Philippines

   Email: keith.vargas@eee.upd.edu.ph

























Vargas                 Expires September 15, 2020               [Page 4]
