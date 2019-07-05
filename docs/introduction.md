---
id: introduction
title: Introduction
sidebar_label: Introduction
---

unmock provides an opinionated and scalable way to mock external services in your tests.

## Motivation

Services are the glue connecting modern applications: for example, the GitHub API is a service. Every service has a specification describing how the service behaves, be either written documentation, [OpenAPI](https://www.openapis.org/), [RAML](https://raml.org/), or something else. The specification for a service should be _reusable_ across applications.

Still, when testing how our applications integrate with external services, we rarely use the specifications directly, but instead write adhoc rules for how the external services should behave. Writing rules is error-prone, time-consuming, and hard to maintain.

This is what unmock wants to fix. Testing the integration with external services should rely on the _service specification_. Setting the service _state_ should happen programmatically before every test. The state should be consistent with the service specification.

## How it works

Let's assume you want to fetch a GitHub organization's email address using `fetch`:

```js
async function fetchGitHubOrganizationEmail(organization) {
  const headers = new Headers({
    Authorization: `Bearer ${process.ENV.GITHUB_API_TOKEN}`,
  });
  const organizationData = await fetch(
    `https://api.github.com/orgs/${organization}`,
    headers
  );
  return organizationData.email;
}
```

Your test looks like this:

```js
it("fetches GitHub organization's email", async () => {
  const email = await fetchGitHubOrganizationEmail("meeshkan");
  expect(email).toBe("contact@meeshkan.com");  // email address for Meeshkan organization
}
```

The test would complete successfully if the `GITHUB_API_TOKEN` environment variable was set properly. However, it is bad practice to hit the API in unit tests, because

1. running unit tests counts against the rate limit,
1. bugs in the code could corrupt production data,
1. organization's email could change and that would break the test.

Let us add unmock to capture outgoing calls and check if the value returned from `fetchGitHubOrganizationEmail` is a `String`:

```js
import unmock from "unmock-node";

beforeAll(() => {
  unmock.initialize();  // Capture outgoing calls
})
it("fetches GitHub organization's email", async () => {
  const email = await fetchGitHubOrganizationEmail("meeshkan");
  expect(email).toEqual(expect.any(String));
}
```

To describe what the GitHub service is, we add a yml file:

```yaml
# __unmock__/github/index.yml

servers:
  - https://api.github.com/

paths:
  /orgs/{org}:
    email: string!
```

Running the test should pass now. If you're familiar with the OpenAPI specification, the contents of `index.yml` should look familiar to you, and indeed the file format is an extension of the OpenAPI specification. Read more about services and their specifications [here](services.md).

The specification for the `github` service is saying that the returned email address should be any string. This may be all you need to test the function logic. If you want the service to return a given email address instead of a random string, simply add a call to `states.github()` to modify the _state_ of the service:

```js
import unmock, { states } from "unmock-node";

beforeAll(() => {
  unmock.initialize();  // Capture outgoing calls
})
it("fetches GitHub organization's email", async () => {
  // Set the email address returned by the GitHub service:
  states.github({ email: "contact@meeshkan.com" });
  const email = await fetchGitHubOrganizationEmail("meeshkan");
  expect(email).toBe("contact@meeshkan.com");
}
afterAll(() => {
  unmock.reset();  // Reset the state
})
```

Get started with unmock [here](getting-started.md).

## Mission

unmock wants to provide a semantically adequate, mocked version of the internet so you never have to create a mock manually again. Before we get there, we want to enforce best practices for testing external APIs.

## Next steps

1. [Get started](getting-started.md) with unmock
1. Read about [services](services.md)
1. Read about [unmock DSL and how to modify the service state](state.md)
