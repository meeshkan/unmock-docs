---
id: introduction
title: Introduction
sidebar_label: Introduction
---

Unmock provides an opinionated, easy way to create and manage mocks for external APIs.

## Motivation

Use unmock to

- develop with confidence: never hit an external API by accident again from your tests
- substitute mocks when your tests try to hit the network
- decorate your tests with `@unmock(...)` to programmatically define mock behaviour (in development)

## How it works

Let's assume you want to fetch a GitHub user's email address using `node-fetch`:

```js
async function fetchGitHubUserEmail(username) {
  const headers = new Headers({
    Authorization: `Bearer ${process.ENV.GITHUB_API_TOKEN}`,
  });
  const userData = await fetch(
    `https://api.github.com/users/${username}`,
    headers
  );
  return userData.email;
}
```

You want to test the logic in your function and write a test looking like this:

```js
it("fetches GitHub user email", async () => {
  const email = await fetchGitHubUserEmail("meeshkan");
  expect(email).toBeDefined();
}
```

You have setup the unmock preset in [Jest](https://jestjs.io/), so you can safely run the tests:

```text
jest
```

Unmock captures the outgoing HTTP call of the form

```json
{
  "host": "https://api.github.com",
  "path": "/v3/users/meeshkan",
  "headers": {
    "Authorization": "Bearer XXX"
  }
}
```

and notices you haven't created a matching mock yet. It immediately fails the test and presents the following options:

```text
No matching mock was found for the call https://api.github.com/v3/users/meeshkan.
Create the mock with "yarn unmock fix" and run the tests again.
```

Typing `yarn unmock fix`, a mock will be created for you in the `__unmocks__` folder and opened in your editor. After filling in the suitable response body and status code, you can save and close the file, and watch the tests pass.

Never hit an external API again without knowing about it!

## Comparison to existing tools

### nock

[Nock](https://github.com/nock/nock) is the de-facto tool for mocking HTTP servers in Node.js and we love it for its amazing list of capabilities. However, we think nock has a few drawbacks:

1. It's easy to accidentally hit real APIs if `nock.disableNetConnect()` is not called before every test
1. It's hard to programmatically declare what mocks should be served for which request.

In contrast, unmock expects that you

1. add a test preset that will ensure you will never hit external network by accident
1. fail your way to success. You run the tests, unmock intercepts the request and knows which request should be mocked, and it's a breeze for you to create the matching mock with a single command.

### yesno

Like unmock, [yesno](https://github.com/FormidableLabs/yesno) is based on [node-mitm](https://github.com/moll/node-mitm) to intercept and mock outgoing network connections.

### Hoverfly

## Mission

unmock wants to provide a semantically adequate, mocked version of the internet so you never have to create a mock manually again. Before we get there, we want to enforce best practices for testing external APIs.

## Next steps

1. [Get started](quick-start.md) with unmock CLI
1. Setup unmock with [Jest](https://jestjs.io/) for ultimate testing experience
