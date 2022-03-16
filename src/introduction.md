## Octocat

Octocat aims to be _the_ GitHub API library for Rust. It was created due to a lack of well-maintained alternatives. 

## Design Overview

* There are two modes of operation for Octocat. 

* The first (and most common) one is as a webhook event listener:
    * Essentially, the event listener passes payloads to the user, who then responds with a [`Command`](https://octocat-rs.github.io/octocat-rs/octocat_rs/github/command/struct.Command.html) that can either contain nothing or a number of futures.
    * The `Command` is executed as soon as the event loop is free, and the user recieves the result in the form of a [`Message`](https://octocat-rs.github.io/octocat-rs/octocat_rs/github/handler/trait.EventHandler.html#associatedtype.Message) (an enum which they define).

~~~admonish info "Visualization"
```
                                             ┌────────────────┐  ┌────────┐
┌──────────────┐  ┌─────┐   ┌─────────────┐◄─┤Result (Message)├──┤        │
│Event Listener├──┤Event├──►│Event Handler│  └────────────────┘  │Executor│
└──────────────┘  └─────┘   └───┬─────────┴─┐                  ┌►│        │
                                │      ▲    └──────────────────┘ └────────┘
                                │      │
                                │  ┌───┴───┐
                                │  │Command│
                                │  └───┬───┘
                                ▼      │
                               ┌───────┴┐
                               │  User  │
                               └────────┘
```
~~~

* The second is as a glorified HTTP client wrapper.
