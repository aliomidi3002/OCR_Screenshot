Here is the updated `README.md` with the **Notes** section moved to the top, right before the installation steps.

````markdown
# Linux Screen OCR & Translation Tool

## Overview
This documentation provides commands and scripts to extract text from screen areas using OCR (Optical Character Recognition) and translate it instantly. It is designed to bridge the gap between your desktop environment and Google Translate, specifically optimized for **English to Farsi** translation.

## Notes & Compatibility
* **Compatibility**: This may not work on every distro or window manager. It relies on specific package managers (apt) and display servers.
* **Wayland**: Uses `grim`/`slurp` for screenshots, supports direct stdin piping.
* **GNOME**: Uses `gnome-screenshot`, requires temporary files but works on both X11 and Wayland.
* **Language codes**: `eng` (English), `fas` (Farsi/Persian), `ara` (Arabic), `spa` (Spanish), etc.
* **Translation**: The commands below open Google Translate with **English to Farsi** translation by default.

---

## Wayland Approach

### Installation
```bash
sudo apt update
sudo apt install tesseract-ocr grim slurp wl-clipboard
````

### Text Extraction Commands

#### Save to Text File

```bash
grim -g "$(slurp)" - | tesseract stdin stdout > ocr_result.txt
```

#### Copy to Clipboard

```bash
grim -g "$(slurp)" - | tesseract stdin stdout | wl-copy
```

#### Farsi/Persian Text Recognition

```bash
grim -g "$(slurp)" - | tesseract stdin stdout -l fas
```

### Translation Command

```bash
temp_var=$(grim -g "$(slurp)" - | tesseract stdin stdout) && firefox "[https://translate.google.com/?sl=en&tl=fa&text=$temp_var&op=translate](https://translate.google.com/?sl=en&tl=fa&text=$temp_var&op=translate)"
```

-----

## GNOME Approach (X11/Wayland Compatible)

### Installation

```bash
sudo apt update
sudo apt install tesseract-ocr gnome-screenshot
# For clipboard operations:
sudo apt install wl-clipboard  # Wayland
# OR
sudo apt install xclip         # X11
```

### Text Extraction Commands

#### Copy to Clipboard

```bash
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout | wl-copy
```

#### Save to Text File

```bash
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout > ocr_result.txt
```

#### Farsi/Persian Text Recognition

```bash
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout -l fas | wl-copy
```

### Translation Command

```bash
gnome-screenshot -a -f /tmp/screenshot.png && firefox "[https://translate.google.com/?sl=en&tl=fa&text=$(tesseract](https://translate.google.com/?sl=en&tl=fa&text=$(tesseract) /tmp/screenshot.png stdout)&op=translate"
```

-----

## Advanced Usage

### Multi-language OCR

```bash
# English + Farsi
grim -g "$(slurp)" - | tesseract stdin stdout -l eng+fas | wl-copy

# GNOME version
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout -l eng+fas | wl-copy
```

### Custom Output File

```bash
# Wayland
grim -g "$(slurp)" - | tesseract stdin stdout > ~/Documents/ocr_output.txt

# GNOME  
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout > ~/Documents/ocr_output.txt
```

### With Error Handling

```bash
# Wayland
grim -g "$(slurp)" - | tesseract stdin stdout && echo "OCR successful" || echo "OCR failed"

# GNOME
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout && echo "OCR successful" || echo "OCR failed"
```

```

### Next Step
Would you like me to write a short `bash` script file (e.g., `install.sh`) that automates that installation process so users don't have to copy-paste the commands one by one?
```
