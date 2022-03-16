## Understanding Builders 

All builders in Octocat follow a similar pattern. Each one implements the [`Builder`](https://octocat-rs.github.io/octocat-rs/github_rest/builders/trait.Builder.html) trait, and contains setters for each of its fields, nested or not.

```admonish warning
Builders and methods are currently limited in their variety. This will change with time, however it may take a while for work to resume.
```

### The `Builder` trait

```rust,ignore,does-not-compile
#[async_trait]
pub trait Builder {
    type Response: DeserializeOwned;

    async fn execute<T>(self, client: &T) -> Result<Self::Response, GithubRestError>
    where
        T: Requester;
}
```

### Examples

~~~admonish example
Commenting on a commit.


```rust,ignore,does-not-compile
let res = CommitCommentBuilder::new()
    .owner("octocat-rs")
    .repo("octocat-rs")
    .sha("fcc8348f8286d05976090a9086d64eefb90e3f8b")
    .body("Some text here")
    .execute(&DefaultRequest::new("TOKEN"))
    .await
    .unwrap();

// Prints the URL at which you can find the comment you've just made.
dbg!(res.html_url);
```
~~~

~~~admonish example
Reacting to a commit comment.

```rust,ignore,does-not-compile
let _ = CommentReactionBuilder::new()
    .owner("octocat-rs")
    .repo("octocat-rs")
    .comment_id(67534661)
    .reaction(Reaction::Rocket)
    .execute(&DefaultRequest::new("TOKEN"))
    .await
    .unwrap();

// Comment you just reacted to: https://github.com/octocat-rs/octocat-rs/commit/40919cbf40530cf15a002d701a1a1bd6a6006105#commitcomment-67534661
```
~~~