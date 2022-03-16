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