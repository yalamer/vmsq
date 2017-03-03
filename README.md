# vmsq [![npm version](https://badge.fury.io/js/vmsq.svg)](http://badge.fury.io/js/vmsq)

query valve's master servers

```sh
$ npm install vmsq
```

## usage

#### `vmsq(masterServer :: String, region :: Number, filters? :: Object) → ReadableStream`

- `masterServer`: The master server to query, e.g. `hl2master.steampowered.com:27011`
- `region`: Region to query, e.g. `0x03` or `vmsq.EUROPE`
- `filters`: Optional object that contains [filters][filters] sent with the query

Returns a [`ReadableStream`][readablestream] emitting each server as a string one by one.

## example

```js
const vmsq = require('vmsq')

const stream = vmsq('hl2master.steampowered.com:27011', vmsq.EUROPE, {
  gamedir: 'cstrike',
  map: 'de_nuke',
  empty: 1
})

const servers = []

stream.on('error', (err) => {
  console.error(err)
})

stream.on('data', (ip) => {
  console.log(ip)
  servers.push(ip)
})

stream.on('end', () => {
  console.log(`got ${servers.length} servers`)
})
```

### output

```
88.241.39.16:27015
178.150.208.1:40000
...
got 24 servers
```

## notes

- The master servers appear to be rate-limited, and sometimes stop replying
  during queries returning large number of servers. This results in a
  timeout error being emitted.
- [Documentation of Master Server Query Protocol](https://developer.valvesoftware.com/wiki/Master_Server_Query_Protocol)

[readablestream]: https://nodejs.org/api/stream.html#stream_readable_streams
[filters]: https://developer.valvesoftware.com/wiki/Master_Server_Query_Protocol#Filter
