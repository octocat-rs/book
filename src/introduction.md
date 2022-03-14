## Octocat

Octocat aims to be _the_ GitHub API library for Rust. It was created due to a lack of well-maintained alternatives. 

## Design Overview

* There are two modes of operation for Octocat. 

* The first (and most common) one is as a webhook event listener:
    <!-- TODO: Hyperlink types to docs -->
    * Essentially, the event listener passes payloads to the user, who then responds with a `Command` that can either contain nothing or a number of futures.
    * The `Command` is executed as soon as the event loop is free, and the user recieves the result in the form of a `Message` (an enum which they define).

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
