# parcel-transformer-unix-timestamp

Transforms runtime-generated Unix timestamps into build-time constants.

## Background

In many business scenarios, different effects need to be activated based on specific time points. For instance, a promotional campaign might start at the beginning of the year, or a special visual effect might be displayed on a website during a holiday season. These time-based triggers are often represented as Unix timestamps, which are numerical values that represent the number of seconds that have passed since the Unix Epoch (January 1, 1970 00:00:00 GMT).

However, Unix timestamps can be hard to read and understand, especially when they are hardcoded in the source code. This is where `dayjs("2024-01-01T00:00:00+08:00").unix()` comes in handy. It provides a human-readable string representation for the timestamp, making the code easier to understand and maintain.

## Advantages

This plugin takes this a step further by transforming these runtime-generated Unix timestamps into build-time constants. This has several advantages:

- Performance Optimization: By calculating the Unix timestamps at build time, the plugin reduces the computational load at runtime. This can lead to faster load times and a smoother user experience.
- Maintainability: The plugin allows developers to write timestamps in a human-readable string format, which improves the readability and maintainability of the code. At the same time, it ensures that the final code delivered to the browser uses the more efficient number format.
- Ease of Use: The plugin is easy to set up and use. It can be added to any project with just a few lines of configuration.

## Usage

This plugin is currently not available as an npm package. However, you can manually set it up in your project by following these steps:

### Step 1: Create the Transformer File

Create a new file named `transformer-unix-timestamp.mjs` in your project root directory. This file will contain the transformer code. Here’s the content:

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

Next, create or update the `.parcelrc` file in your project root directory. This file tells Parcel to use the `transformer-unix-timestamp.mjs` transformer for all `.js`, `.jsx`, `.ts`, and `.tsx` files. Here’s the content:

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
