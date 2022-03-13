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
        * `Client<Self>` is the default value. You shouldn't have to override this unless you're writing your own custom `GitHubClient` implementation (not recommended)
    * This is used to represent the type of the `github_client` parameter present in each event.

```rust,ignore,does-not-compile
#[async_trait]
trait EventHandler {
    type GitHubClient: GitHubClient + Send + Sync;

    ...

    async fn example_event(
        &self,
        github_client: Arc<Self::GitHubClient>,
        example_event: ExampleEventType,
    ) -> Command<Self::Message> {
        Command::none()
    }
    
    ...
}
```

### Listener Configuration

### Supported on all platforms.

* `listener_secret`
    * Requires the `secrets` feature to be enabled.
    * Your webhook secret.

```rust,ignore,does-not-compile
trait EventHandler {
    fn listener_secret(&self) -> &[u8] {
        "secret".as_bytes()
    }

    ...
}
```

### Unsupported on WebAssembly

* `listener_port`
    * The port where the event listener listens (`8080` by default).

```rust,ignore,does-not-compile
trait EventHandler {
    fn listener_port(&self) -> u16 {
        8080
    }

    ...
}
```
* `route`
    * The route at which payloads are to be accepted (`/payload` by default).

```rust,ignore,does-not-compile
trait EventHandler {
    fn route(&self) -> &'static str {
        "/payload"
    }

    ...
}
```

### Events

* There are functions for each possible webhook event, and they all follow the same format. Here's the ping event as an example:

```rust,ignore,does-not-compile
#[async_trait]
trait EventHandler {
    async fn ping_event(
        &self, 
        github_client: Arc<Self::GitHubClient>,
        ping_event: PingEvent,
    ) -> Command<Self::Message> {
        Command::none()
    }

    ...
}
```
### Usage Example

* Here's an example of the `EventHandler` trait in action.

```rust,ignore,does-not-compile
#[derive(Debug)]
struct Handler {}

#[derive(Debug)]
enum Message {
    Stuff(&'static str),
}

#[async_trait]
impl EventHandler for Handler {
    type Message = Message;
    type GitHubClient = Client<Self>;

    fn listener_port(&self) -> u16 {
        2022
    }

    async fn message(&self, message: Self::Message) {
        match message {
            Message::Stuff(s) => {
                println!("==> Message received: {s}");
            }
        }
    }

    async fn commit_event(
        &self,
        github_client: Arc<Self::GitHubClient>,
        commit: PushEvent,
    ) -> Command<Self::Message> {
        println!("Commit pushed!");

        Command::perform(async { "Computation finished" }, Message::Stuff)
    }
}

#[tokio::main]
async fn main() -> Result<()> {
    ClientBuilder::new().event_handler(Handler {}).build()?.start().await;

    Ok(())
}
```
