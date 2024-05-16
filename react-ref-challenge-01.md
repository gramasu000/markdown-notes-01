## Referencing values with refs (Challenge #1)

This challenge is called **"Fix a broken chat input"**.

It is located in the [Referencing values with refs](https://react.dev/learn/referencing-values-with-refs) webpage of the React documentation.

It presents the following code

```javascript
import { useState } from "react";

export default function Chat() {
  const [text, setText] = useState("");
  const [isSending, setIsSending] = useState(false);
  let timeoutID = null;

  function handleSend() {
    setIsSending(true);
    timeoutID = setTimeout(() => {
      alert("Sent!");
      setIsSending(false);
    }, 3000);
  }

  function handleUndo() {
    setIsSending(false);
    clearTimeout(timeoutID);
  }

  return (
    <>
      <input
        disabled={isSending}
        value={text}
        onChange={(e) => setText(e.target.value)}
      />
      <button disabled={isSending} onClick={handleSend}>
        {isSending ? "Sending..." : "Send"}
      </button>
      {isSending && <button onClick={handleUndo}>Undo</button>}
    </>
  );
}
```

The component above is supposed to work as follows.

You have an input text field, and a send button.

If you fill out the input, and click send, there will be three second pause before an alert box pops up on the screen indicating that the input text is sent.

In that three second pause, an undo button is displayed (in addition to the input text and send button which are still there). This undo button is hidden before we click send, and will disappear when the alert box pops up.

If, during the three second pause, we click the undo button, the alert box should not show up - it should "undo" the send. In addition, the undo button should hide and we should be back to just having the input and the send button - the UI elements that we started with.

However, the code as written above does not work as intended.
When we click the undo button during the three second period, we revert back to the starting UI elements, but we still get the alert box popping up letting us know that the input text is sent.

The `setTimeout` function is responsible for initiating the alert box showing after three seconds. `setTimeout` returns a number which is saved in a variable called `timeoutID`, which is then passed to the `clearTimeout` function in the undo button click handler function - which is supposed to undo the `setTimeout` call.

However, `timeoutID` is a normal variable, and in React, normal variables do not persist during subsequent renders of a component.

When the send button is pressed, the `setTimeout` function is called and `timeoutID` is set to the correct value. However, the component _rerenders_ (to show the undo button). During this rerender, `timeoutID` loses its value and is set back to null. Now, when we click the undo button, we are calling `clearTimeout(null)`, which will do nothing to undo the `setTimeout` call. Therefore, we see the alert box pop up even when we clicked undo.

The solution therefore, is to not use a normal variable to hold the `timeoutID`, but to use something that will persist between component rerenders. In other words, we could use a state or a ref. Either will work fine, but I prefer using ref here.

When refs change value, they do not trigger a component rerender. When state changes value, they trigger a component rerender.

In our case, when we store the timeoutID, there is no need to rerender the component again after it has already been rerendered to show the undo button. There is no harm in rerendering again, but it is simply not necessary. Therefore, it makes sense to use a ref to store timeoutID.

Here is the correct code.

```javascript
import { useState, useRef } from "react";

export default function Chat() {
  const [text, setText] = useState("");
  const [isSending, setIsSending] = useState(false);
  const timeoutID = useRef(null);

  function handleSend() {
    setIsSending(true);
    timeoutID.current = setTimeout(() => {
      alert("Sent!");
      setIsSending(false);
    }, 3000);
  }

  function handleUndo() {
    setIsSending(false);
    clearTimeout(timeoutID.current);
  }

  return (
    <>
      <input
        disabled={isSending}
        value={text}
        onChange={(e) => setText(e.target.value)}
      />
      <button disabled={isSending} onClick={handleSend}>
        {isSending ? "Sending..." : "Send"}
      </button>
      {isSending && <button onClick={handleUndo}>Undo</button>}
    </>
  );
}
```

Note: The `useRef` function returns an object like `{ current: null }`, so we have to use `timeoutID.current` instead of just `timeoutID`.

_Author: Gautam Ramasubramanian_

_Last Updated: May 16, 2024_
