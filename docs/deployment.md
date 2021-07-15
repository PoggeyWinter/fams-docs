---
sidebar_position: 5
---

# Deployment

## Application

As the application was built following Test Driven Development guidelines, the [code repository](https://github.com/Jerell/fams) contains unit tests for each component, which must\* pass successfully for the system to be deployed.

We use GitHub Actions to build our code and run automated tests.

:::note

\*It did work this way before I changed from Heroku to Vercel.

At the moment the tests and deployment are independent, but the plan is for code only to be deployed after successful tests. I need to update the GitHub Actions workflow to make that happen.

:::

After passing all tests, the code is deployed with [Vercel](https://vercel.com/).

## Model

[![npm](https://img.shields.io/npm/v/ccs-sim)](https://www.npmjs.com/package/ccs-sim)
[![codecov](https://codecov.io/gh/Jerell/ccs-sim/branch/main/graph/badge.svg?token=IYPFAIVX26)](https://codecov.io/gh/Jerell/ccs-sim)

`ccs-sim` is unit tested with Jest. After passing automated tests, it is published to npm. GitHub Actions is used to produce coverage reports with Codecov.
