# Slack Bots need a REST API
_2016.01.01_

--------------------------------------------------------------------------------

_If any of this sounds like I'm speaking from experience, it's because I've been through all this before with [gifbot](https://github.com/kenhoff/gifbot). Better gifs for Slack, smarter than /giphy!_

--------------------------------------------------------------------------------

Hi there! Today, we'll be talking about Slack's [Real Time Messaging API](https://api.slack.com/rtm), Slack [Bot Users](https://api.slack.com/bot-users), and how much I wish that there was a REST API for them.

Right now, Slack Bot Users use the Real Time Messaging API to communicate with Slack. The Real Time Messaging API is a WebSocket-based API - basically, our server just opens a socket-y connection to Slack's API service, and they're able to receive and send messages back and forth.

## @jokebot: tell me a joke
Let's work through this with an example, called `@jokebot`.

We want `@jokebot` to tell us jokes. Just simple, one-line jokes - nothing that requires a response (like a knock-knock joke). `@jokebot` is a member `#general`. If I (`@kenhoff`) post in `#general`:

> @kenhoff: @jokebot, tell me a joke

Then `@jokebot` will respond with:

> @jokebot: The rotation of earth really makes my day.

(cue collective groan from `#general`)

### Implementation
What actually happens here?

First, I post the message to `#general`. This sends a request to Slack, and updates the channel state.

Then, Slack notifies all of the users that are listening to the channel.

In this case, our bot is in `#general`, and is listening for channel updates. Our bot now gets a message, saying that @kenhoff just posted a message to `#general` with the contents "`@jokebot`, tell me a joke".

_It's important to note that `@jokebot` actually gets all messages from `#general`, not just the ones that start with "`@jokebot`"._

So, Slack sends this message (along with a bunch of metadata) to all clients that are connected to `@jokebot` via WebSocket.

On the server, we get the message, which looks a bit like this (I use [slack-client](https://github.com/slackhq/node-slack-client)):

```javascript
slack.on("message", function(message) {
    // process the message
}
```

_If you print the contents of the message at this point, it'll look a little different. Instead of `@jokebot` you'll probably see something like `<@U123456>`, which is just `@jokebot`'s Slack user ID._

Okay, cool! So we receive the message, check to see if it's directed at us, get a joke, and post the joke back to whatever channel it was requested from.

```javascript
slack.on("message", function (message) {
    // check to see if message was directed at us, @jokebot
    // retrieve a joke from dadjokes.txt
    // post the joke to the channel that `message` was originally sent from
})
```

## @jokebot: post a joke to #developers
Now, let's add another feature. I'd like to be able to _direct message_ `@jokebot`:

> @kenhoff: post a joke to #developers

At which point `@jokebot` will go off and post a hilarious joke to `#developers`:

> ...

And the little green dot next to `@jokebot`'s name in the sidebar disappears, and _it crashes_.

### What happened?
Well, it's a great thing that we've implemented the "error" message handling from [slack-client](https://github.com/slackhq/node-slack-client):

```javascript
slack.on("error", function(err) {
    console.log(err);
}
```

Because we see this in our server output:

```
invalid channel id
```

This isn't user error, and I'm positive that the code is doing what it's supposed to be. We _definitely_ have the right channel ID for `#developers`.

The reason we're getting an error is that `@jokebot` isn't a member of `#developers`.

Even worse, the error message doesn't contain anything more than `invalid channel id`. No information what channel we tried to post to. So, if we had _two_ people trying to post hilarious jokes to `#developers` and `#sales` at the same time, and we get _one_ `invalid channel id` error message, how do we know which channel we don't have access to?

There's a couple ways that we could handle this:
1. We could maintain a list of the most recent requests made by all users. If we receive an error message, then we know it's for the most recent request made by a user. However, this is susceptible to race conditions.
2. We could check and make sure that `@jokebot` is a member of a specified channel before attempting to post to it. However, it only accounts for that specific error message - if there's other types of error messages, we have to handle those server-side as well.
3. We could just have a REST API for posting bot messages. We'd get proper HTTP responses and solve all of our problems.

Of course, just having a REST API to post bot messages doesn't solve the whole problem. There still needs to be a way to receive messages meant for bots, which is where I think having a webhook-based system would be awesome; that way, retries could happen even if a server is temporarily unavailable, as opposed to the "well, guess we'll just drop it" if no servers are connected via WebSocket.

But, two-way communication is absolutely essential with bot users, so my guess is that's why they decided on the WebSocket method.

--------------------------------------------------------------------------------

_I'd love to hear how I should be doing this better! Hit me up on Twitter ([@ken_hoff](https://twitter.com/ken_hoff)) or say hello at [ken@hoff.tech](mailto:ken@hoff.tech)._

--------------------------------------------------------------------------------

_Disclaimer: I've only been working with Slack bots for a couple months now. All of my learnings so far may be completely erroneous, so use caution when developing your own production-ready bots._
