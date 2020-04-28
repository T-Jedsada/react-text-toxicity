## React Text Toxicity

Detect toxicity in text using React.

It's based on this model: https://github.com/tensorflow/tfjs-models/tree/master/toxicity

## Getting started

```
yarn add react-text-toxicity
```

Import as:

```js
import useTextToxicity from "react-text-toxicity";
```

Use it as:

```js
const predictions = useTextToxicity('text', { threshold, delay });
```

* **text** - Text to predict
* **options**
  * **threshold** Prediction threshold *(0.9 as default)*
  * **delay** Delay in milliseconds applied to debounce multiple changes *(300ms as default)*


## Example

![example](./example/toxicity.gif)

```jsx
import React, { Fragment, useState } from "react";
import useTextToxicity from "react-text-toxicity";

function Toxicity({ predictions }) {
  const style = { margin: 10 };

  if (!predictions) return <div style={style}>Loading predictions...</div>;

  return (
    <div style={style}>
      {predictions.map(({ label, match, probability }) => (
        <div style={{ margin: 5 }} key={label}>
          {`${label} - ${probability} - ${match ? "🤢" : "🥰"}`}
        </div>
      ))}
    </div>
  );
}

export default function Index() {
  const [value, setValue] = useState("");
  const predictions = useTextToxicity(value);

  return (
    <div style={{ display: "flex" }}>
      <div>
        <div>Write something</div>
        <textarea
          style={{ width: 300, height: 200 }}
          value={value}
          onChange={(e) => setValue(e.target.value)}
        />
      </div>
      {value && <Toxicity predictions={predictions} />}
    </div>
  );
}
```