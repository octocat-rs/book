## Understanding EventHandler

`EventHandler` is a trait you must implement if you intend to use the event listener portion of Octocat in any meaningful way.

Let's break down what you need to know.

### Associated types

* `Message`
    <!-- TODO: Hyperlink types to docs -->
    * The enum which you recieve when a future from a [`Command`]() is completed.

```rust,ignore,does-not-compile
trait EventHandler {
    type Message: std::fmt::Debug + Send,

    ...

    async fn message(&self, message: Self::Message) {
        {}
    }

    ...
}
```

* `GitHubClient`
    * In 99% of cases, this type should be set equal to `Client<Self>`.
        * `Client<Self>` will be made the default once GATs are stabilized.
    * This is used to represent the type of the `github_client` parameter present in each event.

```rust,ignore,does-not-compile
trait EventHandler {
    type GitHubClient: GitHubClient + Send + Sync;

    ...

    // NOTE: Not representative of what an actual event looks like.
    async fn event(
        &self,
        github_client: Arc<Self::GitHubClient>,
        app_event: EventType,
    ) -> Command<Self::Message> {
        Command::none()
    }
    
    ...
}
```

### Listener Configuration

* `listener_port`
    * The port where the event listener listens (`8080` by default).

```rust,ignore,does-not-compile
trait EventHandler {
    ...

    fn listener_port(&self) -> u16 {
        8080
    }
}
```
* `route`
    * The route at which payloads are to be accepted (`/payload` by default).

```rust,ignore,does-not-compile
trait EventHandler {
    ...

    fn route(&self) -> &'static str {
        "/payload"
    }
}
```
