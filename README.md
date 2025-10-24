# Developer Documentation

This README provides guidance on how to work with awork's Developer Documentation.

## Introduction

awork uses Fern to generate and publish the Developer Docs. They API reference is based on an OpenAPI spec.

## OpenAPI Documentation

The live OpenAPI specification is available at: <https://api.awork.com/openapi/v1>

## Project Structure

The project follows this structure:

```plaintext
fern/
├── docs.yml // The main Fern configuration goes here.
├── apis/
│   └── v1/
│       ├── summary.mdx // Documentation of API v1.
│       └── openapi/
│           └── openapi.yml // OpenAPI spec of API v1.
├── pages
│   ├── api // All API reference pages go here.
│   └── introduction.mdx 
├── assets // All image files and other assets go here.
```

## Previewing the Docs

```sh
fern docs dev
```

## Publishing the Docs

To publish the docs to Fern, run the following command.

```sh
fern generate --docs --log-level debug
```

## Fern Documentation

<https://buildwithfern.com/learn/api-definition/introduction/what-is-an-api-definition>

Icons from <https://fontawesome.com/search>.
