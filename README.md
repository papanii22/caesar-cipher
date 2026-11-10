# Caesar Cipher

A small web page that encrypts and decrypts messages with the Caesar cipher. You type a message, choose a shift key from 1 to 25, and press **Encrypt** or **Decrypt**.

The whole project is one file, `index.html`. It contains the page (HTML), the design (CSS) and the logic (JavaScript). Nothing is installed and nothing is sent to a server: the work happens in the visitor's browser.

## What the Caesar cipher is

The Caesar cipher is one of the oldest ways to hide a message. Every letter is replaced by the letter a fixed number of places further along the alphabet. That fixed number is the **shift key**.

With a shift key of 3:

| Plain  | H | E | L | L | O |
|--------|---|---|---|---|---|
| Cipher | K | H | O | O | R |

When a shift runs past Z it wraps around to the start, so with a key of 3, `X Y Z` becomes `A B C`.

To read the message, the receiver shifts every letter back by the same key.

## How to use the page

1. Type or paste a message into the **Message** box.
2. Enter a **shift key** from 1 to 25.
3. Press **Encrypt** to hide the message, or **Decrypt** to reveal one.

The result appears underneath, showing the text you entered and the converted text.

To check it works, encrypt `Hello, World!` with key 3. You get `Khoor, Zruog!`. Paste that back in, keep the key at 3, and press Decrypt to get the original.

The page shows a message instead of a result if the box is empty or the key is not a whole number from 1 to 25.

## How the code works

The logic is three JavaScript functions at the bottom of `index.html`.

### `encrypt(text, shift)`

Goes through the message one character at a time.

- **Capital letter (A–Z):** turn the letter into its position in the alphabet (A = 0, B = 1 … Z = 25), add the shift, and use the remainder after dividing by 26 so the result wraps around. Then turn the position back into a letter.
- **Small letter (a–z):** the same, starting from `a`, so capitals stay capital and small letters stay small.
- **Anything else** (spaces, numbers, punctuation): left unchanged.

As a formula, where `p` is the letter's position:

```
new position = (p + shift) mod 26
```

Example: `Y` is position 24. With shift 3, (24 + 3) mod 26 = 1, which is `B`.

### `decrypt(text, shift)`

Decrypting means shifting back. Shifting back by `shift` gives the same result as shifting forward by `26 − shift`, because the alphabet is a loop of 26 letters. So `decrypt` reuses `encrypt`:

```js
function decrypt(text, shift) {
  return encrypt(text, 26 - shift);
}
```

Example: a message encrypted with key 3 is decrypted by shifting forward 23. In total each letter has moved 26 places, which brings it back to where it started.

### `handleSubmit(mode)`

Runs when either button is pressed. `mode` is `"encrypt"` or `"decrypt"`.

1. Reads the message and the shift key from the page.
2. Checks the message is not empty and the key is a whole number from 1 to 25. If not, it shows an error and stops.
3. Calls `encrypt` or `decrypt`.
4. Shows the result, with labels that match the button: *Plaintext → Ciphertext* when encrypting, *Ciphertext → Plaintext* when decrypting.

## Design

- **Black and white only.** The page uses pure black (`#000000`) and pure white (`#ffffff`), with no other colours.
- **Minimal layout.** Centred text, small capital labels, no boxes and no border lines.
- **Hover effects that move nothing else.** Text boxes grow slightly and buttons lift a little when the mouse is over them. These use the CSS `transform` property, so the background colour and the position of everything else stay the same.
- **Works on phones.** The layout is one narrow column that fits small screens.

## Limits

The Caesar cipher is for learning, not for real secrets. There are only 25 possible keys, so anyone can try them all in a few seconds. It also keeps the pattern of the language: the most common letter in the encrypted text is usually the most common letter of the original.

The page only shifts the English letters A–Z and a–z. Accented letters, numbers and symbols pass through unchanged.

## Run it

Open `index.html` in any web browser. No setup is needed.

To put it online, host the file on any static hosting service such as Vercel. No build step or settings are required.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The whole app: page structure, styling and cipher logic |
| `README.md` | This explanation |
