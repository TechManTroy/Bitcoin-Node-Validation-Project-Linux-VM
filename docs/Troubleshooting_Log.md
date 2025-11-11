# Troubleshooting Log

## Issue T-01: Non-Standard Installation Media Compression

| Category | Networking/Setup |
| :--- | :--- |
| **Phase/Step** | Phase 0, Step 1 (Media Acquisition) |
| **Date** | 2025-11-11 |

### Problem (P)
The official pfSense ISO image was downloaded as a multi-layered compressed archive (.gz), which was not recognized by the Windows host OS's native file extractor. Attempts to use the compressed file resulted in a failure to mount in VirtualBox.

### Action (A)
1.  **Specialized Tool Acquisition:** Installed the open-source decompression utility **7-Zip** to handle the `.gz` file format, demonstrating reliance on specialized tools for system utility.
2.  **Multi-Step Extraction:** Used 7-Zip's **Extract Files** function to perform the two-step decompression necessary to reveal the final usable **`.iso`** image (e.g., extracting from `.gz` to an intermediate file, then to the final `.iso`).

### Result (R)
The usable `.iso` installation file was successfully acquired. This established a robust workflow for handling non-standard archives, validating the integrity of the media before proceeding with the installation.
