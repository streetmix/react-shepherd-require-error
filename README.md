# react-shepherd 7.0.6 `require` error repro

Importing `react-shepherd@7.0.6` in an ESM environment (Vitest, or a Parcel dev build) throws:

> Error: Calling `require` for "react" in an environment that doesn't expose the `require` function.

Cause: `dist/index.js` inlines React's jsx-runtime (CJS) and, since the move to Rolldown, calls `__require("react")` (line ~1908) instead of importing it.

## Reproduce
```
npm install
npm test        # fails on 7.0.6
npm i react-shepherd@7.0.5 && npm test   # passes
```

Suggested fix: mark `react`, `react/jsx-runtime`, `react/jsx-dev-runtime` as external in the build.
