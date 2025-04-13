# Key Distribution and Agreement

**Keys** are secret values which are used to encrypt and decrypt messages. But before two people or systems can communicate securely, they need to share these keys. This specific problem comes under **Key Distribution and Agreement**. Without key distribution or key agreement, it would be hard to communicate securely online. These methods helps with,

- Keeping messages private
- Make online banking, communication, and shopping safe
- Protect data from unauthorized adversary

## Key Distribution

**Key Distribution** means sending a secret key from one person or system to another in a secure manner. This method works best when there is a trusted third party or secure channel between 2 parties to help with the process.

**Example:** Alice wants to communicate securely with Bob. A trusted server shares the same secret key to both Alice and Bob. Now, both of them have the same key and can communicate securely.

## Key Agreement

**Key Agreement** is when both people (or systems) work together to create a shared secret key. No one sends the full key directly. Instead, they exchange some public information and calculate the same key on both sides.

**Example:** Alice and Bob both agrees on some public numbers. They each select a secret number and mix it with the public numbers. They send the mixed results to each other. Using some math, they both get the same secret key — even though they didn’t send it directly!

## References

- [Wikipedia – Key Distribution](https://en.wikipedia.org/wiki/Key_distribution)
- [Wikipedia – Key Agreement](https://en.wikipedia.org/wiki/Key-agreement_protocol)
