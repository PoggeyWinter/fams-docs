---
sidebar_position: 5
---

# Deployment

As the application was built following Test Driven Development guidelines, the [code repository](https://github.com/Jerell/fams) contains unit tests for each component, which must\* pass successfully for the system to be deployed.

We use GitHub Actions to build our code and run automated tests.

:::note

\*It did work this way before I changed from Heroku to Vercel.

At the moment the tests and deployment are independent, but the plan is for code only to be deployed after successful tests. I need to update the GitHub Actions workflow to make that happen.

:::

After passing all tests, the code is deployed with [Vercel](https://vercel.com/).
