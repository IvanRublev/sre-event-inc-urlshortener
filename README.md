# sre-event-inc-urlshortener 

Running application is on https://sre-event-inc-urlshortener.fly.dev/

# Architecture Canvas

## Value proposition

The application shortens, persists, and redirects to URLs provided by user. 

## Key Stakeholder

Internet users authenticated at the website.

## Core Functions

* Register and Sign In a user by email and password
* Accepts an URL (f.e. https://google.com) from an authenticated (signed in) user and returns a shortened version that looks like https://sre-event-inc-urlshortener.fly.dev/elevenlettercode
* Redirects to the URL with the shortened version for non-authenticated users
* Supports editing the shortened URL with https://sre-event-inc-urlshortener.fly.dev/edit/elevenlettercode for authenticated users

## Quality Requirements

* Scalability to handle peak loads
* Cost-effective performance maintenance strategies
* Robust security practices to protect data and prevent unauthorised actions

## Business Context

* The system persists shortened URLs in the PostgreSQL application `sre-event-inc-urlshortener-db` on Fly.io
* Running the system on Fly.io's trial plan may affect the response times of the system

## Core Decisions

+ Reusing existing open source project https://github.com/SphericalKat/katbin
+ Use CI/CD pipeline with GitHub Actions
- Low test coverage inherited

## Technologies

* Elixir Phoenix/Ecto for webservice
* bcrypt_elixir/Rustler for password hashing
* PostgreSQL for data persistance
* Github Actions for deployment
* Fly.io for hosting

## Components/Modules

```
+-----------+    +------------+
| Webserver |----| PostgreSQL |
+-----------+    +------------+
```

Main webserver Routes:

| Method | Path | Purpose | Authentication? |
| ------ | ---- | ------- | -------------- |
| GET | /    | Home page, to Register or Sign In | No |
| GET | /elevenlettercode/raw | Shows the full URL | No |
| GET | /v/elevenlettercode | Shows the shortened URL | No |
| GET | /elevenlettercode | Redirects to the full URL | No |
| GET | /edit/elevenlettercode | Edit the shortened URL | Yes |

## Risks and Missing Information

* Application doesn't collect redirection metrics
* Application doesn't have a real-time dashboard displaying usage metrics


# CI/CD Pipeline and Deployment

Deployments are made in canary mode: old instances shut down only when new instance launches successfully.

GithubActions workflow:

* .github/workflows/fly.yml
* .github/workflows/test-branch.yml

Fly.io schema

* ./fly.toml

## Deployment rules

* Direct pushes to master are prohibited. You can make changes in a new branch, then make a PR to master
* PR can be mered to master only when all tests pass

## Times

* Tests for a new branch takes ~1min
* Deployment to production takes ~2 min

# Local Setup

To start your Phoenix server:

  * Copy `config/dev.secret.sample.exs` to `config/dev.secret.exs`
  * Fill in the SMTP and database configuration in `config/dev.secret.exs`
  * Install dependencies with `mix deps.get`
  * Create and migrate your database with `mix ecto.setup`
  * Install Node.js dependencies with `npm install` inside the `assets` directory
  * Start Phoenix endpoint with `mix phx.server`

Now you can visit [`localhost:4000`](http://localhost:4000) from your browser.

# Additional Documentation

  * Original repository: https://github.com/SphericalKat/katbin
  * Fly.io documentation: https://fly.io/docs
  * GithubActions documentation: https://docs.github.com/en/actions
  * Libraries docs: https://hexdocs.pm
  * Forum: https://elixirforum.com/c/phoenix-forum
