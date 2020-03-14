



CoE 151                                                        K. Vargas
Internet-Draft                                                      EEEI
Updates: 1 (if approved)                                  March 14, 2020
Intended status: Standards Track
Expires: September 15, 2020


           Server-Client and Peer-Peer Communication Protocol
                          coe-151-mp1-protocol

Abstract

   This document describes the communication protocol of the server-
   client model and peer-peer model.  This will not cover rules and
   restrictions for implementations of the control logic and processing
   of user input.  Message formats are described along with their use
   cases.  Handshake / login, state transitions, and terminations are
   also described below.

Disclaimer

   This only covers the communication protocol.  Implementations of
   control logic may vary.

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
   to this document.  Code Components extracted from this document must
   include Simplified BSD License text as described in Section 4.e of
   the Trust Legal Provisions and are provided without warranty as
   described in the Simplified BSD License.

1.  Introduction

   Chat servers are simple applications if written by a single entity.
   The lack of implementation rules adds complexity on ensuring that two
   applications communicate with each other properly.

2.  References

2.1.  Normative References

   [RFC2119]  Bradner, S., "Key words for use in RFCs to Indicate
              Requirement Levels", BCP 14, RFC 2119,
              DOI 10.17487/RFC2119, March 1997,
              <https://www.rfc-editor.org/info/rfc2119>.

2.2.  Informative References

   [RED]      Floyd, S. and V. Jacobson, "Random Early Detection (RED)
              gateways for Congestion Avoidance", IEEE/ACM Transactions
              on Networking 1(4) 397--413, August 1993,
              <http://www.aciri.org/floyd/papers/early.pdf>.

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







Vargas                 Expires September 15, 2020               [Page 2]
