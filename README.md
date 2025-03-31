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

### Details

This seems to have been introduced with Yarn 4.3.0 with https://github.com/yarnpkg/berry/pull/6205.

#### Output on Yarn `4.2.2` ✅

```
➤ YN0000: · Yarn 4.2.2
➤ YN0000: ┌ Resolution step
➤ YN0000: └ Completed
➤ YN0000: ┌ Post-resolution validation
➤ YN0002: │ repro-yarn-optional-peers-required@workspace:. doesn't provide react (p18cd3), requested by @material-ui/core.
➤ YN0002: │ repro-yarn-optional-peers-required@workspace:. doesn't provide react-dom (p2cc76), requested by @material-ui/core.
➤ YN0086: │ Some peer dependencies are incorrectly met; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code.
➤ YN0000: └ Completed
```

#### Output on Yarn `4.3.0` ❌

```
➤ YN0000: · Yarn 4.3.0
➤ YN0000: ┌ Resolution step
➤ YN0000: └ Completed
➤ YN0000: ┌ Post-resolution validation
➤ YN0002: │ repro-yarn-optional-peers-required@workspace:. doesn't provide @types/react (p7178c), requested by @material-ui/core.
➤ YN0002: │ repro-yarn-optional-peers-required@workspace:. doesn't provide react (pa20c8), requested by @material-ui/core.
➤ YN0002: │ repro-yarn-optional-peers-required@workspace:. doesn't provide react-dom (pe9d33), requested by @material-ui/core.
➤ YN0086: │ Some peer dependencies are incorrectly met by your project; run yarn explain peer-requirements <hash> for details, where <hash> is the six-letter p-prefixed code.
➤ YN0000: └ Completed
```
