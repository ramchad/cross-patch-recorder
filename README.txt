Cross Patch Recorder BLE v3
============================

WHAT THIS VERSION DOES
- Uses Web Bluetooth to connect to your nRF52840 through Nordic UART Service.
- Subscribes to TX notifications:
  Service UUID: 6e400001-b5a3-f393-e0a9-e50e24dcca9e
  TX UUID:      6e400003-b5a3-f393-e0a9-e50e24dcca9e
- Expects 200-byte frames:
  2-byte header + 196-byte payload + 2-byte trailer
- Decodes 98 little-endian uint16 payload values as:
  CH1, CH2, CH1, CH2, ... = 49 samples/channel
- Uses the same Python-style scaling:
  (uint16 - 32768) * 100 / 65536
- Shows live CH1 and CH2.
- Manual Start/Stop recording.
- Event markers.
- Exports CSV and metadata JSON.

IMPORTANT
This page MUST be hosted over HTTPS and opened inside Bluefy on iPhone/iPad.
Opening the local .html file from Files is not enough for Web Bluetooth.

FIRST TEST
1. Host index.html on GitHub Pages / Netlify / Cloudflare Pages.
2. Open the HTTPS URL in Bluefy.
3. Power on the patch.
4. Tap Connect Patch.
5. Select the nRF52840 device.
6. Confirm live CH1/CH2.
7. Record only 10-30 seconds.
8. Stop.
9. Share/Save CSV.
10. Open the CSV and verify values before doing a long experiment.

CSV
Columns:
time_sec, CH1, CH2, packet_index

CH1/CH2 are the decoded values matching the existing Python scaling, not final input-referred mV.
Your current MATLAB scaling can therefore remain responsible for conversion to input-referred mV.

LIMITATION
The app shows "framing issues", not true dropped packets.
True packet-loss detection requires us to verify what the firmware's 2-byte header/trailer contain
and whether there is a packet sequence counter.

LONG RECORDINGS
This v3 stores decoded CSV rows in browser memory until export.
Use short tests first. After BLE is verified, the next version should use chunked/IndexedDB storage
for robust 30-minute to multi-hour recordings on iPhone.
