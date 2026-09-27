# Mini PE Analyzer & String Extractor

<img width="900" height="593" alt="image" src="https://github.com/user-attachments/assets/831c10f7-48f0-4505-840c-71b570f1091e" />


I built this because I wanted a quick, zero-install, browser-based tool to peek inside binary files and check for basic Windows PE structures or suspicious network APIs without firing up heavy tools. 

*Note on String Extraction:* Since this runs entirely as a client-side JavaScript script parsing raw bytes, the primitive ASCII string extraction has limitations. It pulls out consecutive printable characters, but it doesn't handle wide-character (UTF-16 Unicode) strings properly yet, meaning many PE file strings might appear garbled or missing. It's a lightweight proof-of-concept rather than a full-fledged disassembler!

## Features

- **Zero-Install & Serverless:** Runs completely inside your browser via GitHub Pages. Drag and drop any `.exe` or `.bin` file to inspect it instantly.
- **PE Header Detection:** Instantly checks for the DOS/Windows `MZ` magic header to verify if the file is a valid Portable Executable.
- **Keyword & API Scanning:** Scans the raw binary content for common network and system indicators (e.g., `ws2_32.dll`, `wininet.dll`, `http://`, `CreateProcess`).

## How It Works

- **Architecture & Workflow:**
  - **File Ingestion:** When you drop a binary file into the drop zone, the browser reads it into memory as an `ArrayBuffer` using the `FileReader` API.
  - **Header & Pattern Matching:** The script inspects the first few bytes (`Uint8Array`) to match the `0x4D5A` (`MZ`) signature. It then converts portions of the buffer to text to run keyword lookups against known network-related DLLs and protocols.
  - **Raw String Parsing:** Iterates through byte values, filtering for standard printable ASCII ranges (`32` to `126`) to harvest readable sequences of characters.
