# repro-yarn-optional-peers-required

This repository reproduces an issue with Yarn 4.8.1 where unsatified optional peer dependencies log a YN0002 warning.
E.g.,

```
➤ YN0002: │ repro-yarn-optional-peers-required@workspace:. doesn't provide @types/react (p7178c), requested by @material-ui/core
```

## Steps to reproduce

1. Clone the repository
2. Run `yarn`

Notice the warning about the unsatisfied optional peer dependency `@types/react` requested by `@material-ui/core`.

Checking the `package.json` of `@material-ui/core`, we can see that it has a peer dependency on `@types/react` with the optional flag set to true.

```json
{
  "peerDependencies": {
    "@types/react": "^16.8.6 || ^17.0.0",
    "react": "^16.8.0 || ^17.0.0",
    "react-dom": "^16.8.0 || ^17.0.0"
  },
  "peerDependenciesMeta": {
    "@types/react": {
      "optional": true
    }
  }
}
```
