# VoidLab

Privacy-first tools. No accounts, no tracking, no servers storing your data.

## What is this

VoidLab builds free tools for file sharing and communication that don't require trusting a third party. Your data goes directly from point A to point B.

## Tools

### Transfer

Peer-to-peer file transfer with no size limit.

Files are sent directly between browsers using WebRTC DataChannels. Nothing passes through our servers.

**How it works:**
1. Sender generates an offer code
2. Receiver pastes it and generates an answer
3. Sender pastes the answer
4. Direct connection established, transfer begins

## Security

### Encryption layers

| Layer | Protocol | Purpose |
|-------|----------|---------|
| Key exchange | X25519 (ECDH P-256 fallback) | Derive shared secret |
| Key derivation | HKDF-SHA-256 | Generate encryption key from shared secret |
| Data encryption | AES-256-GCM | Encrypt file chunks with integrity check |
| Transport | DTLS 1.2+ | WebRTC's built-in transport security |

### Implementation details

- **Chunk size:** 64KB with backpressure control
- **IV:** Unique 12-byte IV per chunk
- **Salt:** Random 32-byte salt per session
- **Keys:** Ephemeral, destroyed after session ends
- **Verification:** SHA-256 fingerprint displayed for manual comparison

### What we don't protect against

- IP addresses are visible to peers (use Tor Browser for anonymity)
- No protection against compromised endpoints
- No audit from external security firm (yet)

## Tech stack

- Vanilla HTML/CSS/JS
- Web Crypto API
- WebRTC DataChannel
- No build step, no dependencies

## Run locally

```bash
# Clone
git clone https://github.com/vj88-coder/securechat.git
cd securechat

# Serve (any static server works)
python3 -m http.server 8000
# or
npx serve .
```

Open `http://localhost:8000/transfer.html`

Note: WebRTC requires HTTPS in production. Localhost works for testing.

## Project structure

```
├── index.html        # Landing page
├── transfer.html     # Transfer tool (includes app logic)
├── manifeste.html    # Project manifesto
└── README.md
```

All tools are single-file HTML with embedded CSS and JS. No build process.

## Contributing

Open an issue or PR. Security reports welcome.

Code style: keep it simple, no frameworks, no build tools.

## License

MIT
