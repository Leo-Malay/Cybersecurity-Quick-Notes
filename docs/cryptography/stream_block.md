# Stream & Block Cipher

## Stream Cipher

**Stream Cipher** is a type of encryption where plain text is encrypted one bit or byte at a time. It generates a keystream (a sequence of pseudorandom bits) that is combined with the plaintext, typically using the XOR operation, to produce the cipher text.

- They are lightweight and fast which makes them ideal for real-time applications.
- They operate on continuous streams of data, rather than breaking it into blocks.
- Security depends heavily on the randomness and secrecy of the keystream.
- Initialization vectors (IVs) are often used to ensure different keystreams even with the same key.

**Example:** Alice and Bob are on a secure voice call. As Alice speaks, her words are turned into a stream of data. The stream cipher encrypts this data on-the-fly before sending it. Bob, using the same keystream generator and key, decrypts the stream in real-time to hear Alice clearly. Anyone trying to eavesdrop will only hear unintelligible noise.

## Block Cipher

A **Block Cipher** encrypts fixed-size blocks of plaintext (for example, 64 or 128 bits) using a symmetric key. If the length of plain text is larger than a single block, it’s divided and encrypted in chunks, with each block processed separately or in relation to the previous one depending on the mode of operation.

- Common block sizes include 64-bit (e.g., DES) and 128-bit (e.g., AES).
- Various modes of operation like ECB, CBC, and CTR determine how blocks are encrypted in sequence.
- More suitable for data at rest (e.g., files, databases) due to structured processing.
- Generally offers stronger security guarantees when properly configured.

**Example:** Alice wants to store her personal journal securely on her laptop. She might use a block cipher to encrypt the file. The data is broken into 128-bit blocks, and each block is encrypted using the same key, ensuring the entire journal is protected even if someone gains access to her storage.

## References

- [Wikipedia – Stream Cipher](https://en.wikipedia.org/wiki/Stream_cipher)
- [Wikipedia – Block Cipher](https://en.wikipedia.org/wiki/Block_cipher)
