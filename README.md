# Quasar Colspans

Example of Quasar Table with colspans and rowspans.
Source data like that:

```json
{
  "id": 1,
  "userData": {
    "name": "Alex",
    "surname": "Ronin",
    "age": 35
  },
  "task": "task 1",
  "month": {
    "plan": 10,
    "close": 20
  }
}
```

will be shown as table with multi-level header:
![multi-level header](/QuasarColspans_screenshot.jpg "multi-level header")

---

### Install the dependencies

```bash
yarn
# or
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)

```bash
quasar dev
```

### Lint the files

```bash
yarn lint
# or
npm run lint
```

### Format the files

```bash
yarn format
# or
npm run format
```

### Build the app for production

```bash
quasar build
```

### Customize the configuration

See [Configuring quasar.config.js](https://v2.quasar.dev/quasar-cli-vite/quasar-config-js).
