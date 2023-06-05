<h1 align="center">pg-listen-pid</h1>
<h3 align="center">pg-listen fork that exposes postgres processId</h3>

<p align="center">
  <a href="https://travis-ci.org/andywer/pg-listen">
    <img alt="Build Status" src="https://travis-ci.org/andywer/pg-listen.svg?branch=master" />
  </a>
  <a href="https://www.npmjs.com/package/pg-listen">
    <img alt="NPM Version" src="https://img.shields.io/npm/v/pg-listen.svg" />
  </a>
</p>

<br />

A fork of [`pg-listen`](https://github.com/andywer/pg-listen) that exposes the processId from eventemitter.


## Installation

```sh
# using npm:
npm install pg-listen-pid
```

## Usage
```sh
import createSubscriber from "pg-listen-pid"
import { databaseURL } from "./config"

// Accepts the same connection config object that the "pg" package would take
const subscriber = createSubscriber({ connectionString: databaseURL })

subscriber.notifications.on("my-channel", (payload, processId) => {
  // Payload as passed to subscriber.notify() (see below)
  console.log("Received notification in 'my-channel':", payload, "with PID:", processId)
})

subscriber.events.on("error", (error) => {
  console.error("Fatal database connection error:", error)
  process.exit(1)
})

process.on("exit", () => {
  subscriber.close()
})

export async function connect () {
  await subscriber.connect()
  await subscriber.listenTo("my-channel")
}

export async function sendSampleMessage () {
  await subscriber.notify("my-channel", {
    greeting: "Hey, buddy.",
    timestamp: Date.now()
  })
}
```
For more details see [`pg-listen`](https://github.com/andywer/pg-listen).


## License

MIT
