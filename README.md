<div align="center">

![cloudmate logo](https://avatars.githubusercontent.com/u/69299682?s=200&v=4)

# JSON-LD Helper

<small style="opacity: 0.7;">by Cloudmate</small>

---

![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)

</div>

## Why use this library?

When receive JSON-LD,

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "name": "juunini",
  "id": "juunini"
}
```

is equals

```json
{
  "@context": "https://www.w3.org/ns/activitystreams",
  "as:name": "juunini",
  "@id": "juunini"
}
```

and it also equals

```json
[
  {
    "https://www.w3.org/ns/activitystreams#name": [
      {
        "@value": "juunini"
      }
    ],
    "@id": "juunini"
  }
]
```

when receive any shape of JSON-LD, we can parse and use same interface.

## Installation

```sh
# npm
npm install @cloudmatelabs/jsonld-helper

# yarn
yarn add @cloudmatelabs/jsonld-helper

# pnpm
pnpm add @cloudmatelabs/jsonld-helper

# bun
bun add @cloudmatelabs/jsonld-helper
```

## Usage

```ts
import { JsonLDReader } from '@cloudmatelabs/jsonld-helper'

const jsonld = await JsonLDReader.parse({
  "@context": [
    "https://www.w3.org/ns/activitystreams",
    {
      "manuallyApprovesFollowers": "as:manuallyApprovesFollowers"
    }
  ],
  "as:id": "https://mastodon.social/users/juunini",
  "as:type": "Person",
  "as:url": "https://mastodon.social/@juunini",
  "as:image": {
    "as:type": "Image",
    "as:mediaType": "image/png",
    "url": "https://files.mastodon.social/accounts/headers/109/408/471/076/954/889/original/f4158a0d06a05763.png"
  },
  "as:manuallyApprovesFollowers": "true"
})

const imageURL = jsonld
  .setNamespace({ as: 'https://www.w3.org/ns/activitystreams' })
  .read('as', 'image')
  .read('as', 'url')
  .stringOrThrow()
// https://files.mastodon.social/accounts/headers/109/408/471/076/954/889/original/f4158a0d06a05763.png

const id = jsonld.read('@id').get()
// https://mastodon.social/users/juunini

const manuallyApprovesFollowers = jsonld
  .setNamespace({ as: 'https://www.w3.org/ns/activitystreams' })
  .read('as', 'manuallyApprovesFollowers')
  .booleanOrElse(false)
// true
```

## Methods and Properties

```ts
JsonLDReader.parse (value: object | object[], options?: Options.Expand): Promise<JsonLDReader>

.value [readonly]: unknown
.length [readonly]: number

.read (key: string | number): JsonLDReader
.read (namespace: string, key: string): JsonLDReader

.get (): unknown
.getOrThrow (error?: Error): unknown
.getOrElse (defaultValue: unknown): unknown

.stringOrThrow (error?: Error): string
.stringOrElse (defaultValue: string): string

.numberOrThrow (error?: Error): number
.numberOrElse (defaultValue: number): number

.booleanOrThrow (error?: Error): boolean
.booleanOrElse (defaultValue: boolean): boolean
```

## License

[MIT](./LICENSE)

But, this library use [jsonld].  
[jsonld] is licensed under the [BSD-3-Clause License](https://github.com/digitalbazaar/jsonld.js/blob/main/LICENSE)

[jsonld]: https://github.com/digitalbazaar/jsonld.js