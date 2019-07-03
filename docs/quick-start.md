---
id: quick-start
title: Quick start
sidebar_label: Quick start
---

Note: unmock documentation uses yarn, but npm will also work. You can compare yarn and npm [here](https://yarnpkg.com/en/docs/migrating-from-npm#toc-cli-commands-comparison).

1. Clone `unmock/nodejs-quick-start` repository:

   ```text
   git clone https://github.com/unmock/nodejs-quick-start
   cd nodejs-quick-start
   ```

1. Enter `yarn [install]` to install dependencies and `yarn test` to run the tests

1. One of the tests fails, because no matching mock is found for the outgoing network call. Enter `yarn unmock fix` to fix it and add the following body to `response.body`:

   ```json
   // response.json
   {
       "response": {
           "body": {
               "email": "dev@meeshkan.com
           }
       }
   }
   ```

1. Run the tests again and see them pass!
