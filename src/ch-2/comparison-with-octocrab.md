## Comparison with Octocrab

[Octocrab](https://github.com/XAMPPRocky/octocrab) is the closest competitor in the Rust ecosystem to Octocat. There are, however, some major differences between the two. Let's go over them:

* Octocrab is an API Client, whereas Octocat can serve as multiple things, including an API client and event listener. 
* Octocrab's API is currently more intuitive and polished than Octocat's, however it's not as comprehensive.

```admonish info
We are working on the polish, however it will take some time before we can say that we're on-par with the competition here.
```

* Octocat has support for Cloudflare Workers, whereas Octocrab does not.

* Octocat's API is also designed to be much more flexible, something that can be seen in how our builders are structured. 
    * The [`GitHubClient`](https://octocat-rs.github.io/octocat-rs/octocat_rs/github/client/trait.GitHubClient.html) and [`Requester`](https://octocat-rs.github.io/octocat-rs/github_rest/trait.Requester.html) traits also demonstrate this.

```admonish todo 
Detailed comparisons and examples are coming soon; it'll take time for me to become familiar enough with their API to write them.
```

### Direct API Comparisons

```admonish warning
This section is still a WIP.
```

~~~admonish example "Example: Getting 50 issues from a repository"
#### Octocrab

```rust,ignore, does-not-compile
octocrab::instance()
    .issues("microsoft", "vscode")
    .list()
    .per_page(50)
    .send()
    .await?;
```
#### Octocat

```rust,ignore, does-not-compile
# use octocat_rs::{
#   rest::builders::{
#       Builder, GetIssuesBuilder
#   },
#   HttpClient, 
# };
#
let http_client = HttpClient::new_none();

GetIssuesBuilder::new()
    .owner("microsoft")
    .repo("vscode")
    .per_page(50.to_string())
    .execute(&http_client)
    .await?;
```
~~~

As you can see from this example, there is a key difference between how builders function in Octocrab and in Octocat. 

* In Octocrab, the HTTP client is the source of the builder.
* In Octocat, the builder is instantianted separately, with the HTTP client only attached when it's time to execute it. We made this choice because we believe it's more flexible than the alternative; it allows for users to conditionally attach authorization to a request.

Speaking of authorization, let's compare how you would go about adding authorization to an HTTP client in the two.

~~~admonish example "Example: Attaching Authorization to an HTTP Client"
#### Octocrab

```rust,ignore, does-not-compile
# use octocrab::Octocrab;
# 
Octocrab::builder().personal_token("TOKEN").build()?;
```
#### Octocat

```rust,ignore, does-not-compile
# use octocat_rs::{HttpClient, Authorization}
#
let username: String;
let token: String;

let auth = Authorization::PersonalToken { username, token };

HttpClient::new(Some(auth), None);
```
~~~

You might notice that Octocat requires the username whereas Octocrab does not. This is due to a difference in internal design choices, however support for the former option would be easy to implement in Octocat if it is desired. 
