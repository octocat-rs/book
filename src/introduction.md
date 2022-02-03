## Octocat

Octocat aims to be _the_ GitHub API library for Rust. It was created due to a lack of well-maintained alternatives. 

## Design

* There are two modes of operation for Octocat. 

* The first (and most common) one is as a webhook event listener:

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

