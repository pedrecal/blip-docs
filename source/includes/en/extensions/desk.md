## Desk

The **desk** extension allows routing and exchange of messages and notifications between bot users and human agents. The human agents can reply the messages using any BLiP supported help desk application (BLiP Desk for instance). Hence, a bot can forward received messages from users to a **human agent** on the chosen help desk application and vice versa, in a transparent way.

This feature is useful for **enabling humans to reply some complex or unhandled messages as the bot**. For example, imagine that you want a chatbot that knows a lot about soccer teams, but for some reason it doesn't know exactly the tickets prices for some matchs. In this scenario, you can use a human to help the bot answer only when users ask about ticket prices.

*Note: BLiP offers [BLiP Desk](https://desk.blip.ai), a free and powerful desk application to enable humans to reply messages sent by a bot.*

Before using this extension, check if you have already properly set a customer service tool (help desk application) on the Portal and if you already have at least one available human agent to receive and reply to messages.

### Forwarding received messages to a human agent

>Imagine a scenario where a user on Messenger channel asks for human help service. Therefore, while the ticket is still open, any message received by the bot should be sent to a **human agent**.

At first, the bot receives a message and decides if it must route the user to a human agent. Imagine for instance that the message **"Hello, I would like to talk to an attendant."** is enough to send the user to an agent.

```
{
    "id": "1",
    "from": "1654804277843415@messenger.gw.msging.net",
    "to": "bot@msging.net/instance",
    "type": "text/plain",
    "content": "Hello, I would like to talk to an attendant."
}
```

To foward a received message to an agent, send the message to **{encoded-user-node}@desk.msging.net**, where

**{encoded-user-node}** is the ASCII-encoded messages' emmiter node.

```
{
    "id": "1",
    "from": "bot@msging.net/instance",
    "to": "1654804277843415%40messenger.gw.msging.net@desk.msging.net",
    "type": "text/plain",
    "content": "Hello, I would like to talk to an attendant."
}
```

### Forwarding received messages from a human agent to a final user

>Imagine a scenario where a human agent is replying to some message to a user on Messenger channel. The message received by the bot from the human agent must be fowarded to the final user.

First, the bot receives a message as below:

```
{
    "id": "2",
    "from": "1654804277843415%40messenger.gw.msging.net@desk.msging.net",
    "to": "bot@msging.net/instance",
    "type": "text/plain",
    "content": "Hello, here is a human being ;)"
}
```

To forward a received message to the specific final user, the bot must decode the received message node so it knows where to respond **{encoded-user-node}@desk.msging.net**:

```
{
    "id": "2",
    "from": "bot@msging.net/instance",
    "to": "1654804277843415@messenger.gw.msging.net",
    "type": "text/plain",
    "content": "Hello, here is a human being ;)"
}
```

### Handling the end of an attendance

>When the human agent closes some attendance the bot receives a message with a *Redirect* content type. The Redirect's context property has a *Ticket* with information about the attendance. In order to get a closed attendance information, add a receiver to **application/vnd.lime.redirect+json** content type.

```
{
    "id": "1",
    "to": "54f1dd2e-42d2-43f2-9100-68fbbabb9c83@tunnel.msging.net",
    "type": "application/vnd.lime.redirect+json",
    "content": {
        "context": {
            "type": ""type": "application/vnd.iris.ticket+json",
            "value": {
                "id": "1654804277843415",
                "sequentialId": 0,
                "ownerIdentity": "bot@msging.net",
                "customerIdentity": "1654804277843415@messenger.gw.msging.net",
                "agentIdentity": "ravpacheco%40gmail.com@blip.ai",
                "status": "ClosedAttendant",
                "storageDate":"2018-03-20T20:41:54.330Z",
                "externalId":"3cf18133-7b0f-47d2-8719-bbaec6ee14e4",
                "rating":0,
                "team":"Default",
                "unreadMessages":0
            }
        }
    }
}
```

After receiving the Redirect message, the bot can change the user state and start to handle next messages automatically.

### Closing an attendance actively

TBD