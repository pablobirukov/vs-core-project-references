# vs-core-project-references

### File Structure

```
├── tsconfig.json [*1]
├── packages
│   ├── animal
│   │   ├── src
│   │   │   ├── animal.spec.ts
│   │   │   └── animal.ts
│   │   ├── tsconfig.impl.json  [*2]
│   │   └── tsconfig.spec.json  [*3]
│   ├── core
│   │   ├── src
│   │   │   ├── utils.spec.ts
│   │   │   └── utils.ts
│   │   ├── tsconfig.json       [*4]
│   │   └── tsconfig.spec.json  [*5]
│   └── tree
│       ├── package.json
│       ├── src
│       │   ├── tree.spec.ts
│       │   └── tree.ts
│       ├── tsconfig.json       [*6]
│       └── tsconfig.spec.json  [*7]
```

There is a bunch of packages, each of them is a bunch of composite projects (one for implementation files, one for test files). Root [tsconfig](./tsconfig.json) (*1) has empty `files` and references all the nested composite projects (*2...*6).

`animal` and `tree` both depend on `core` package.

There is no tsconfig with default name in [packages/animal](./packages/animal).

Each config in both `animal` ([tsconfig.impl.json](./packages/animal/tsconfig.impl.json), [tsconfig.spec.json](./packages/animal/tsconfig.spec.json)) and `tree` ([tsconfig.json](./packages/tree/tsconfig.json), [tsconfig.spec.json](./packages/tree/tsconfig.spec.json)) has a `paths` alias pointing to `core` package.

`tsc -b` works with no errors.

**Expected behaviour:**

VS Code infers the configs from `references` and doesn't show errors

**Actual behaviour:**

*Presumably, VS Code is looking for a config with a default name, thus in [packages/animal/src/animal.ts](packages/animal/src/animal.ts):*

* error in `Cannot find module 'core/src/utils'`
* error attempting `> TypeScript: Go to project configuration` → `File is not part of a TypeScript project`

*while, there is no errors in [tree/src/tree.ts](tree/src/tree.ts) as there is tsconfig.json*