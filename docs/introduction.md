---
id: introduction
title: Introduction
sidebar_label: Introduction
---

Unmock provides an opinionated, easy way to create and manage mocks for external APIs.

## Motivation

## How it works

Let's assume you want to fetch a GitHub user's email address using `fetch`:

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

## Comparison to existing tools

### nock

### yesno

### Hoverfly

## Mission

unmock wants to provide a semantically adequate, mocked version of the internet so you never have to create a mock manually again. Before we get there, we want to enforce best practices for testing external APIs.

## Next steps

1. [Get started](getting-started.md) with unmock CLI
