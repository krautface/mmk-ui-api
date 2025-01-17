# Merry Maker 2.0

Merry Maker is a fully scalable tool to detect the presence of digital skimmers.

## Background

Merry Maker is a solution designed to detect the presence of digital skimmers, built by two Target security
developers, @cawalch and @ebrandel.

Fundamentally, Merry Maker operationalizes three key processes:

- Preserving a baseline of existing pages by saving the code being served by a website along with the
  network traffic generated by test transactions
- Scanning the saved code for any malicious indicators
- Scanning the saved network traffic for any potential compromise

Merry Maker continually simulates online browsing and completes test transactions to scan for the presence of
malicious code. It acts like a guest on Target.com by completing several typical activities, including
online purchases. While doing so, the tool gathers and analyzes a variety of information, including network requests,
JavaScript files, and browser activity to look for any type of unwanted activity. Merry Maker was built to execute on
all of this at scale.

Merry Maker's purchases are flagged as test orders internally so that they don't get processed, but otherwise,
everything happens behind the scenes just as it normally would during check out. If any possible malicious activity is
detected, Merry Maker triggers an alert to Target's 24/7 Cybersecurity Incident Response Team to prompt an
investigation.

Since its launch in 2018, Merry Maker has completed over one million website scans and we've filed multiple patent
applications. The technology helps keep the holiday shopping season safe and merry here at Target (hence the name).

We have open sourced the Merry Maker framework along with several detection rules in the hopes that this
information helps other cybersecurity teams stand up their own customized defense.

## Features

- [Puppeteer](https://pptr.dev/) scripts to simulate user interactions
- Yara rules for static analysis
- Hooks native JavaScript function calls for detection and attribution
- Near real-time browser event detection and alerting
- Distributed event scanning (rule engine)
- Role based UI with local and OAuth2 authentication options


## Related Projects

- [mmk-js-scope](https://github.com/target/mmk-js-scope) Enumerates javascript requests and hooks native function calls
  with Headless Chrome for use by Merry Maker
- [mmk-types](https://github.com/target/mmk-types) Shared typings between services - only needed for developers


## Full Stack Demo

```
# Start all the services
docker compose -f docker-compose.all.yml up
```

Navigate to `http://localhost:8080` to begin.

## Requirements

- docker
- node v14.18.1

## Setup

### Docker Stack

Includes `postgres`, `redis` and a `testRedis` instance

```
# from ./
docker compose up -d
```

### Backend

API service for the `frontend` and `scanner`

DB Migration

```
# from ./backend
yarn migrate
```

```
# from ./backend
yarn install

yarn start
```

Testing

```
yarn test
```

Uses nodemon to auto reload on change. Listens on two separate HTTP ports (UI and transport)

### Frontend

Vue dev server for developing the frontend. Run `backend` prior to starting this service

```
# from ./frontend
yarn install
yarn serve
```

### Jobs

Main scheduler for running scans, purging old data, and misc cron jobs

```
# from ./backend
yarn jobs
```

### Scanner

Rules runner for processing browser events emitted by `jsscope`

```
# from ./scanner
yarn install
yarn start
```

Testing

```
yarn test
```

### Optional Auth Strategy

#### OAuth2

```
export MMK_AUTH_STRATEGY=oauth
export MMK_OAUTH_AUTH_URL=http://oauth-server/auth/oauth/v2/authorize
export MMK_OAUTH_TOKEN_URL=https://oauth-server/auth/oauth/v2/token
export MMK_OAUTH_CLIENT_ID=client_id
export MMK_OAUTH_SECRET=<oauth-secret>
export MMK_OAUTH_REDIRECT_URL=http://localhost:8080/api/auth/login
export MMK_OAUTH_SCOPE=openid profile email
```

---

```
Copyright (c) 2021 Target Brands, Inc.
```
