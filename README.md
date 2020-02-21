# CoE 151 Machine Problem 1 Chat Server Network Protocol

## Model
    Server
        Client
        Client
        Client

## Serverbound Data
-   RequestUsername
-   RequestUserInfo
-   RequestLocalTime
-   SetUsername
-   Whisper
-   GeneralChat
-   ListPeople
-   Login
-   Disconnect
-   KeepAlive

## Custom Serverbound Data
-   SetAdmin
-   KickClient
-   Ignore
-   Mute

## Clientbound Data
-   ResponseUsername
-   ResponseUserInfo
-   ResponseLocalTime
-   ConfirmUsername
-   UsernameChangeEvent
-   Whisper
-   GeneralChat
-   ResponseListPeople
-   LoginConfirm
-   LoginReject
-   Kick

## Peer-Peer
