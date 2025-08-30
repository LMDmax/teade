# Teade — Maintained Fork (of `zivost/teade`)
IPC (Inter-Process Communication) package using RPC over HTTP.

> This is a community-maintained fork of **zivost/teade** under the MIT License.  
> It is not affiliated with or endorsed by the original authors.

---

## Status & Compatibility

- **Maintenance:** Active (maintained fork)
- **Node.js support:** targets modern LTS (>= **16.x**).  
  The legacy guidance from upstream is kept for historical users:
  - If you use **Node < 4.0**, stick to upstream version **`teade@0.0.6`** (stable). Install with:
	```sh
	npm i -S teade@0.0.6
	```
- **Protocol:** HTTP/HTTPS
- **License:** MIT (see [License](#license))

If you need features from newer versions and are on an older Node, please open an issue; we’re happy to discuss safe backports or alternatives.

---

## Why do you need it?
You probably need this for communicating with different microservices in a microservice architecture.

The most trusted and tested protocol is HTTP and this package uses HTTP to create a Client–Server connection so it can call remote procedures in a reliable manner.

### Features

- Reliability as it uses the HTTP protocol.
- Few dependencies.
- Can be used over **HTTPS** if required.
- Scalable — Use with an internal load balancer or let Teade choose a random port.

### Roadmap / TODO

Go cross‑platform. We are working on `go` and `.netcore` versions. Once done you can connect your servers and clients written in different languages.

If you need it in some other language or want us to speed up the process, please open an issue and help us test. Other feature requests are welcome via issues.

---

## Installation

Choose one of the following depending on how this fork is published in your environment:

### Option A — Scoped package (recommended)
If this fork is published under your org scope (example only):
```sh
npm install --save @your-org/teade
```

### Option B — GitHub install
Install directly from this fork’s repository (replace with your org/repo and tag/branch):
```sh
npm install --save github:your-org/teade#vX.Y.Z
```

### Option C — Original package (upstream)
If you intentionally want the original upstream package (see compatibility note above):
```sh
npm install --save teade
```

---

## Usage

Require/import the library:

```js
// CommonJS
const teade = require('teade')            // or: require('@your-org/teade')

// ESM
// import teade from 'teade'              // or: import teade from '@your-org/teade'
```

### Server

```js
let server = new teade.Server()

server.addService({
  readRPC: read
})

function read(call, callback) {
  myReadProceduralCall(call, function (err, response) {
	if (err) {
	  const error = new Error()
	  // anything attached to error is sent as is
	  callback(error)
	} else {
	  callback(null, response)
	}
  })
}

server.bind(8080)
server.start()
```

### Client

```js
let client = new teade.Client('http://localhost', 8080)

// payload should always be valid JSON
let payload = {
  name: 'John'
}

client.request('readRPC', payload, function (err, response) {
  if (err) {
	// handle error
	// accessible data:
	//  - err
  } else {
	// do something with the response
	// accessible data:
	//  - response
  }
})
```

#### Client Parameters

| Name            | Description                                                           | Example                                               |
|-----------------|-----------------------------------------------------------------------|-------------------------------------------------------|
| Host            | Hostname where the RPC server is running, **prefix the protocol**     | `http://localhost`                                    |
| Port            | Port at which the RPC server is running                               | `8080`                                                |
| Host–Port Set   | Array of `host:port` strings. Teade will choose a random entry.       | `[ "http://localhost:8080", "http://localhost:8081" ]` |

#### Example
See the `test/` directory for now.

---

## Upgrading from upstream

We aim for drop‑in compatibility where possible.

1. Review **CHANGELOG.md** for differences from `zivost/teade`.
2. Update your dependency to the forked package (see **Installation** options).
3. Run your tests and smoke tests.
4. Open an issue if you encounter migration friction—include Node version, logs, and a minimal repro if possible.

---

## Security

If you believe you’ve found a vulnerability, **do not** open a public issue with details.  
Please follow responsible disclosure:
- Open a **private security advisory** in GitHub (preferred), or
- Email the maintainers at **security@your-org.tld** (replace with your alias).

We will acknowledge and coordinate a fix and release.

---

## Contributing

We welcome issues and PRs! Before contributing, please read:
- **CONTRIBUTING.md** — dev setup, coding standards, testing, release process
- **CODE_OF_CONDUCT.md** — community expectations

PRs should include tests and update docs where applicable.

---

## Governance & Maintainers

- **Maintained by:** Your Org / Team (replace with real names/handles)  
- **Decision‑making:** Maintainer review via PRs

If you’re a downstream consumer and want to co‑maintain, please open an issue to discuss.

---

## License

Licensed under the **MIT License**. See **LICENSE**.

### Notice

```
This product includes software originally developed by the authors of “teade”
(c) Original Contributors. Licensed under the MIT License.

Modifications by <Your Org/Team> (c) 2025 <Your Org/Team> and contributors.
This fork is not affiliated with, or endorsed by, the original authors.
```
