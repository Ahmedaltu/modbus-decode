# ModbusDecode — RTU Frame Tool

> A free, browser-based Modbus RTU frame builder, decoder, and reference tool for firmware developers.

**Live:** [modbus-decode.pages.dev](https://modbus-decode.pages.dev) &nbsp;|&nbsp; No install. No signup. No backend.

---

## What is it?

ModbusDecode helps firmware developers work with Modbus RTU frames without memorizing byte structures or doing CRC math manually.

Whether you're reading a CO2 sensor over RS-485, writing values to a PLC, or debugging why your frame is being rejected — this tool handles it in seconds.

---

## Features

### Build Frame
Generate valid Modbus RTU frames from human-readable inputs.

- **Read Registers (FC03)** — enter slave address, register address, and quantity
- **Write Register (FC06)** — enter slave address, register address, and value
- CRC16 auto-computed and appended
- One-click copy to clipboard
- Visual byte breakdown with color-coded fields

### Decode Frame
Paste any raw hex frame and get a full breakdown.

- Supports any valid Modbus RTU frame
- Accepts spaces, dashes, commas, or `0x` prefixed input
- CRC16 validation with computed vs received comparison
- Field-by-field explanation (address, function code, data, CRC)
- FC03 register details (starting address + quantity)
- Educational notes on every field

### What is Modbus?
Built-in reference for beginners and students.

- What is Modbus RTU and when is it used?
- Frame structure explained byte by byte
- How CRC16 works and why it matters
- Master vs Slave explained
- Where to get real frames (serial monitors, logic analyzers, firmware logs)

---

## Supported Function Codes

| Code | Name |
|------|------|
| FC01 | Read Coils |
| FC02 | Read Discrete Inputs |
| FC03 | Read Holding Registers |
| FC04 | Read Input Registers |
| FC05 | Write Single Coil |
| FC06 | Write Single Register |
| FC15 | Write Multiple Coils |
| FC16 | Write Multiple Registers |

---

## Tech Stack

| Layer | Tool |
|-------|------|
| Frontend | Pure HTML, CSS, JavaScript |
| Hosting | Cloudflare Pages |
| Fonts | JetBrains Mono, Syne (Google Fonts) |
| Dependencies | None |
| Backend | None |

Everything runs in the browser. No data is sent anywhere.

---

## CRC16 Implementation

Uses the standard Modbus CRC16 algorithm:

```javascript
function crc16(bytes) {
  let crc = 0xFFFF;
  for (let i = 0; i < bytes.length; i++) {
    crc ^= bytes[i];
    for (let j = 0; j < 8; j++) {
      crc = (crc & 1) ? (crc >> 1) ^ 0xA001 : crc >> 1;
    }
  }
  return crc;
}
// Returns low byte first (little-endian), as per Modbus RTU spec
```

---

## Running Locally

No build step required.

```bash
git clone https://github.com/Ahmedaltu/modbus-decode.git
cd modbus-decode
# Open index.html in your browser
```

Or with a local server:

```bash
npx serve .
# Visit http://localhost:3000
```

---

## Running Tests

Open `tests.html` in your browser to run the CRC16 and frame parsing test suite.

Tests cover:
- CRC16 computation for all 5 example frames
- Hex string parsing (spaces, dashes, commas, 0x prefix)
- Full frame CRC validation (valid and invalid frames)

---

## Deploying to Cloudflare Pages

1. Fork or clone this repo
2. Go to [Cloudflare Pages](https://pages.cloudflare.com)
3. Connect your GitHub repo
4. Build settings: **none** (static site)
5. Deploy

---

## Roadmap

- [x] Frame decoder with CRC validation
- [x] Frame builder (FC03, FC06)
- [x] Educational reference sidebar
- [ ] FC16 Write Multiple Registers builder
- [ ] C code export (generate ready-to-use C structs)
- [ ] Register map builder
- [ ] WebSerial API — connect directly to RS-485 adapter in browser

---

## Background

Built by a firmware engineering student who got tired of manually computing CRC bytes and cross-referencing datasheets while working with Vaisala sensors (GMP252, HMP60) over Modbus RTU on FreeRTOS.

---

## License

MIT — free to use, fork, and modify.

---

*ModbusDecode — built for firmware developers, by a firmware developer.*
