# :robot: GitHub Apps Cheat Sheet :sparkles:

A cheat sheet for GitHub Apps...

## Contents
* [Key concepts](#key-concepts)
* [Resources](#resources)
* [Notable APIs for GitHub Apps](#notable-apis-for-github-apps)
* [Tools](#tools)
* [Best practices](#best-practices)

## Key concepts
* `GitHub Apps`
  * Offer a secure way for third parties to programmatically access protected resources on GitHub
  * Are a first-class actor on GitHub -- acting independently of resource owners (GitHub users and organizations)
  * Have a fine-grained permissions model -- customers are able to more confidently grant third parties access to their protected resources
  * Have dedicated rate limits, that scale with the app's usage
  * Facilitate webhook event consumption
  * Follow a repo-centric permissions model through _installations_
  * Work across GitHub.com and GitHub Enterprise Server

* **Other key terms**:
  * `Installation`: Connects a GitHub App to one or more repositories owned by an organization or user
  * `Permissions`: Dictate what an App can see, or do in the context of an installation
  * `Webhooks`: Dictate what events an App will be notified about, over a single HTTP endpoint, registered with the GitHub App
  * `Server-to-server token`: (Also commonly referred to as the `installation token`, or `installation access token`) Permits access to resources within the scope of an installation, expires after one hour, created via [the REST API](https://developer.github.com/v3/apps/#create-a-new-installation-token)
  * `User-to-server token`: Permits access to resources that are visible to _both_ an end-user _and_ the GitHub App, acquired through an OAuth-_like_ flow
  * `JWT`: ([JSON Web Tokens](https://jwt.io/)) an open web standard, allowing for information to be securely transmitted between two parties as a JSON object, in this context, JWTs are used to securely transmit a _signature_ to GitHub.com to confirm to GitHub that we are the App we are claiming to be

## Resources
* [GitHub Developer Documentation](https://developer.github.com/)
* GitHub [REST](https://developer.github.com/v3/) and [GraphQL](https://developer.github.com/v4/) APIs
* [Migrating OAuth Apps to GitHub Apps](https://developer.github.com/apps/migrating-oauth-apps-to-github-apps/)

## Notable APIs for GitHub Apps
* GitHub App information
  * [Get the authenticated GitHub App](https://developer.github.com/v3/apps/#get-the-authenticated-github-app) (`JWT`)
* Identify installation information
  * [List installations](https://developer.github.com/v3/apps/#list-installations) (`JWT`)
  * [Get an organization installation](https://developer.github.com/v3/apps/#get-an-organization-installation) (`JWT`)
  * [Get a user installation](https://developer.github.com/v3/apps/#get-a-user-installation) (`JWT`)
* Token creation / revocation
  * [Create a new installation token](https://developer.github.com/v3/apps/#create-a-new-installation-token) (`JWT`)
  * [Revoke an installation token](https://developer.github.com/v3/apps/installations/#revoke-an-installation-token) (`installation access token`)
* Identify installation resources
  * [List repositories](https://developer.github.com/v3/apps/installations/#list-repositories) (`installation access token`)
* Identify user-accessible resources
  * [List installations for a user](https://developer.github.com/v3/apps/installations/#list-installations-for-a-user) (`user-to-server OAuth access token`)
  * [List repositories accessible to the user for an installation](https://developer.github.com/v3/apps/installations/#list-repositories-accessible-to-the-user-for-an-installation) (`user-to-server OAuth access token`)

## Tools
* [Octokit libraries](https://developer.github.com/v3/libraries/)
* [`github-apps-helper` plugin](https://github.com/swinton/insomnia-plugin-github-apps-helper) for [Insomnia](https://insomnia.rest)
* [API route specifications](https://github.com/swinton/github-rest-apis-for-insomnia) for [Insomnia](https://insomnia.rest)
* [Probot](https://probot.github.io/)
* [smee.io](https://smee.io/) to test webhooks

## Best practices

**Do:**
* :white_check_mark: Use [webhooks](https://developer.github.com/webhooks/) to ingest data
* :white_check_mark: Cache and re-use server-to-server (installation access tokens) as much as possible
* :white_check_mark: Use [conditional requests](https://developer.github.com/v3/#conditional-requests) wherever possible
* :white_check_mark: Retry requests when handling "fresh" data
* :white_check_mark: Include a descriptive [User-Agent header](https://developer.github.com/v3/#user-agent-required) in your API requests
* :white_check_mark: Save the `X-GitHub-Request-Id` response header value, especially for error (4xx, 5xx) responses
* :white_check_mark: Subscribe to [this RSS feed](https://developer.github.com/changes.atom) for Platform updates
* Consider listing your GitHub App on [GitHub Marketplace](https://github.com/marketplace)
* Consider other best practices listed [here](https://developer.github.com/v3/guides/best-practices-for-integrators/)

**Don't:**
* :x: Depend on concurrent requests, this can trigger [secondary rate limits](https://developer.github.com/v3/guides/best-practices-for-integrators/#dealing-with-abuse-rate-limits)
* :x: Poll, use webhooks where possible
