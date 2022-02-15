# Keys API

To interact with the **Keys API**, all HTTP requests should be prefixed with `/api/keys`.
For example,

```url
http://127.0.0.1:3000/api/keys
```

1. Basic MFS interaction

- `GET /` - Check whether Pinata keys were already set.
- `POST /` - Set Pinata keys

---

# Usage

### 1. Check whether Pinata keys were already set.

Check whether Pinata keys were already set.

```
GET /
```

#### Params

- none

#### Response

- is_missing - True if any one of the Pinata keys are missing.

### 2. Set Pinata keys

Set Pinata Keys. This will throw an exception, if keys are already set.

```
POST /
```

#### Params

- pinata_api `string` - The Pinata API key.
- pinata_secret `string` - The Pinata secret key.

#### Response

- none
