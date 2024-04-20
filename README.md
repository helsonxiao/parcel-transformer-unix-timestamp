# parcel-transformer-unix-timestamp

Transforms runtime-generated Unix timestamps into build-time constants.

## Advantages

The main advantage of this plugin is performance optimization. By transforming Unix timestamps at build time, it reduces the computational load at runtime. This can lead to faster load times and a smoother user experience.

## Usage

This plugin is currently not available as an npm package. However, you can manually set it up in your project by following these steps:

### Step 1: Create the Transformer File

Create a new file named transformer-unix-timestamp.mjs in your project root directory. This file will contain the transformer code. Here’s the content for `transformer-unix-timestamp.mjs`:

```js
import dayjs from "dayjs";
import { Transformer } from "@parcel/plugin";

const REGEX = /(moment|dayjs)\("([^"]*)"\)\.unix\(\)/g;

export default new Transformer({
  async transform({ asset, logger }) {
    let source = await asset.getCode();
    source = source.replace(REGEX, (match, _g1, g2) => {
      logger.log({ message: `transforming ${match}` });
      return dayjs(g2).unix();
    });
    asset.setCode(source);
    return [asset];
  },
});
```

### Step 2: Update Parcel Configuration

Next, create or update the `.parcelrc` file in your project root directory. This file tells Parcel to use the `transformer-unix-timestamp.mjs` transformer for all `.js`, `.jsx`, `.ts`, and `.tsx` files. Here’s the content for .parcelrc:

```
{
  "extends": "@parcel/config-default",
  "transformers": {
    "*.{js,jsx,ts,tsx}": [
      "./transformer-unix-timestamp.mjs",
      "..."
    ]
  }
}
```

By following these steps, you can use this plugin in your project without needing to install it as an npm package. Hope this helps! 😊
