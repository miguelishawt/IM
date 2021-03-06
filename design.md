# Anax IM Application Design - v1.0

# Program Design

The IM application is split up into two applications, one is a server and one is a client. 

A brief description of both is described below.

## Client

A client is a person that is wishing to join a server to communicate with other clients

## Server

Manages all clients that connect to a server


# API

## Classes

>**NOTE:** 
>This is just notes to quickly jot down ideas on what the interfaces for each class MAY look like, although it may be very different to what it actually is.
	
- Room; Contains various participants in the chat
	- int MaxRecentMessages = 100
	- add(Participant)
	- send(Message)
	- getMessages() : string[]

- Participant
	- ParticipantDelegate* delegate
	- get/setName()

- ParticipantDelegate
	- onMessageReceived(Participant& who, Message& msg)
	- onParticipantEntered(Participant& who)
	- onParticipantLeft(Participant& who)

- Message
	- const int MAX_MSG_SIZE = 500
	- Message(string& msg, Time& time = Time::Now)
	- get/setBody(string)
    - get/setTimeSent(Time& time)

- ClientApplication; Handles the client side of the application
	- Participant user
	- CommandProcessor cmdProcessor
	- join(Server)
	- leave()
	- init()
		```
		cmdProcessor.setCommandIdentifier("/");
		cmdProcessor.add(&join, "join");
		cmdProcessor.add(&leave, "leave");
		cmdProcessor.add(&user.setName, "setName");
		```
	- processLine(string line)
        ```
		if(cmdProcessor.isCommand(line))
		{
			cmdProcessor.process(line);
		}
		else
		{
			sendToServer(line);
		}
        ```

- ServerApplication; Hosts a server, so you can chat :P
	- Room

- Command - a signal/slot thingy :P
	- CommandProcessor
		- add(string id, Command)
		- remove(string id)
		- doesContain(string id)
		- process(string command)
		- isCommand(string)
		- set/getCommandIdentifier(string) = "/"

- CommandDelegate
	- onCommandIsProcessed(const Command& command, const std::string& arguments)
