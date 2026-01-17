# PeerDrop Web ⚡

**PeerDrop Web** is a standalone, serverless, peer-to-peer (P2P) file
transfer and video chat application contained entirely within a single
HTML file. It uses **WebRTC** to establish a direct, encrypted
connection between two devices without uploading data to an intermediate
cloud server.

> **Privacy First:** Data flows directly from device to device. No
> sign-ups, no tracking, and no file size limits (dependent on device
> RAM/Storage).

## ✨ Key Features

-   📂 **File & Folder Transfer:**

    -   Send individual files or entire directory structures.
    -   **Resume Capability:** Auto-resume interrupted transfers,
        recovering from lost ACKs or network drops.
    -   **Large File Support:** Uses the Origin Private File System
        (OPFS) and *WritableStreams* to handle large files efficiently
        without crashing browser memory.
    -   **Batching:** Queue multiple files and accept/reject them in
        bulk.

-   🔒 **Secure P2P Chat:** End-to-end encrypted text messaging that
    persists only for the session duration (refreshing destroys
    history).

-   📹 **Video Calls & Screen Sharing:**

    -   High-quality video/audio calls with mute controls.
    -   Desktop screen sharing (Sender only).
    -   **Mobile Optimizations:** Picture-in-Picture and custom
        pinch-to-zoom/pan logic for video streams on mobile devices.

-   🚀 **Zero Installation:** Runs entirely in the browser (PWA
    Installable).

-   📶 **Network Diagnostics:** Real-time telemetry, speed graphs,
    latency checks, and detailed connection logs.

-   🔗 **Universal Handshake:** Connect via **QR Code** (camera) or
    **Link/Text** (manual copy-paste) for devices without cameras.

## 🛠️ How It Works

PeerDrop Web utilizes **WebRTC** to create a direct data pipe. Because
there is no signaling server to orchestrate the connection, the devices
must perform a manual \"Handshake\" to exchange connection details
(SDP):

1.  **Offer:** The **Host** generates a code containing their
    IP/Port/Encryption keys.
2.  **Answer:** The **Guest** scans the Host\'s code and generates a
    response.
3.  **Verify:** The **Host** scans the Guest\'s response to finalize the
    connection.

Once connected, the traffic travels via Local LAN (highest speed) or WAN
(if devices are on different networks), facilitated by public STUN
servers.

## 🚀 Usage Guide

### 1. Installation / Deployment

PeerDrop is a **Single File Application**.

-   **Run Locally:** Download the *.html* file and open it in your
    browser.

    -   Note: For P2P to work across different devices, the file must be
        served over **HTTPS** or **localhost**.

-   **Deploy:** Upload the single *.html* file to GitHub Pages, Netlify,
    Vercel, or any static web host.

### 2. Establishing a Connection

1.  Open PeerDrop on Device A (Host) and Device B (Guest).

2.  **Device A:** Click **\"Start Room (HOST)\"**. A QR code will
    appear.

3.  **Device B:** Click **\"Join Room (GUEST)\"**.

    -   **Camera Mode:** Scan Device A\'s QR code.
    -   **Manual Mode:** If cameras fail, click \"Use Manual Mode\" to
        copy/paste the connection strings via a messenger app
        (WhatsApp/Signal/Slack).

4.  **Device B** will generate an Answer QR Code.

5.  **Device A** scans Device B\'s Answer QR.

6.  **Connected!** ⚡

### 3. Sending Files

-   **Drag & Drop:** Drag files anywhere onto the screen (Chrome
    Desktop/Android).
-   **Buttons:** Use the 📄 (File) or 📁 (Folder) buttons in the chat
    bar.
-   **Mobile:** On Chrome Android, use Split Screen to drag files from
    your file manager directly into the browser.

## ⚠️ Detailed Limitations

The application relies on browser-specific APIs (WebRTC, File System
Access API), leading to the following constraints:

### 1. iOS (iPhone/iPad) Restrictions

-   **Screen Sharing:** iOS blocks outgoing Screen Sharing entirely.
-   **File Saving:** Safari on iOS does not support the File System
    Access API, meaning it cannot save files automatically to disk.
    Users must manually tap \"Download\" or \"Save to Device\" for each
    file.
-   **Background Activity:** iOS aggressively freezes background tabs.
    If you switch apps for more than \~20 seconds, the connection will
    likely drop.

### 2. Android / Mobile Browser Issues

-   **Firefox Mobile:** Does not support Drag & Drop API.
-   **Backgrounding:** Switching apps (multitasking) generally kills the
    WebRTC connection on mobile devices to save battery. The app
    includes a \"WakeLock\" to keep the screen on, but minimizing the
    app risks disconnection.
-   **Memory:** If the File System Access API is unavailable (e.g.,
    Private Browsing), files are buffered in RAM. A safety check
    prevents downloading files larger than 50% of available RAM to avoid
    crashing the tab.

### 3. Network & Connectivity

-   **No TURN Server:** The app uses only public STUN servers
    (*stun.l.google.com*). If both users are behind strict Symmetric
    NATs (e.g., corporate firewalls or some 4G/5G networks), the
    connection may fail because there is no relay server.
-   **Manual Re-Handshake:** Because there is no central server, if the
    connection drops completely (beyond the auto-reconnect window),
    users must perform the QR code handshake again.
-   **HTTPS Required:** Cameras and Microphones are blocked by browsers
    on insecure (HTTP) origins unless strictly on *localhost*.

### 4. File System

-   **OPFS:** The Origin Private File System (used for large files) is
    often disabled in Incognito/Private windows, causing transfers to
    fall back to RAM buffering (which has size limits).
-   **Folder Structure:** While folder transfer is supported, restoring
    the exact directory structure relies on the receiver\'s browser
    supporting *showDirectoryPicker* (Chromium-based browsers).

## ⚙️ Advanced Configuration

Access the **Menu (☰)** for advanced tools:

-   **Beginner Mode:** Toggles simplified UI vs. detailed controls.
-   **Network Config:** Manually select which network interface (IP) to
    use (useful for forcing LAN over VPN).
-   **Diagnostics:** View raw WebRTC logs (*VERBOSE* level) and
    real-time bandwidth graphs.

## 📦 Dependencies

The application loads the following libraries via CDN (included in the
HTML):

-   *qrcode.min.js*: For generating QR codes.
-   *jsQR*: For scanning QR codes via video stream.
-   *lz-string*: For compressing connection data (SDP) to make QR codes
    smaller.

## ⚠️ Disclaimer

This software is provided \"as is\". While data is encrypted in transit
via standard WebRTC protocols, no warranty is implied. Large file
transfers rely on browser implementation of streams and may vary by
device memory (RAM).

**License:** MIT / Open Source
