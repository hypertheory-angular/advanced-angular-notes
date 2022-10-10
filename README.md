# Advanced Angular Development

## Setup

### Creating the NX Workspace

```shell
npx create-nx-workspace@latest
```

For Questions:

- Workspace name (e.g. org name): **ht**
- What to create in new workspace: (Arrown down to Angular, hit enter)
- Application Name: (frontend)
- CSS
- No Distributed Caching

### Prettier

```shell
npm i prettier-plugin-multiline-arrays
```

The `.prettierrc` config:

```json
{
  "plugins": ["./node_modules/prettier-plugin-multiline-arrays"],
  "singleQuote": true,
  "singleAttributePerLine": true,
  "trailingComma": "all",
  "bracketSameLine": true,
  "arrowParens": "always"
}
```

### ESLint

In the .eslintrc.json file, add the `eqeqeq: "warn"` rule.

### Tailwind

Run the NX Generator to setup tailwind.

```shell
npm i -D @tailwindcss/typography @tailwindcss/forms daisyui
```

Update the `tailwind.config.js` file:

```js
const { createGlobPatternsForDependencies } = require("@nrwl/angular/tailwind");
const { join } = require("path");

/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    join(__dirname, "src/**/!(*.stories|*.spec).{ts,html}"),
    ...createGlobPatternsForDependencies(__dirname),
  ],
  theme: {
    extend: {},
  },
  plugins: [
    require("@tailwindcss/typography"),
    require("@tailwindcss/forms"),
    require("daisyui"),
  ],
};
```
