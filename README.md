## Project Description

This project provides simple screenshot to text extraction using Tesseract OCR on Linux environments.
It supports both Wayland and GNOME approaches for capturing and processing text from any selected screen area.
Users can save extracted text to files, copy it to the clipboard, or perform OCR in multiple languages.
It also includes optional commands for quick translation of recognized text through a web browser.

## Note

This project may not work on every distro or window manager.
Make sure required tools like Tesseract, screenshot utilities and clipboard managers are installed.
Wayland and GNOME methods behave differently based on the environment, so results can vary.

## Wayland Approach

### Installation
```bash
sudo apt update
sudo apt install tesseract-ocr grim slurp wl-clipboard
```
### Language Installation Guide
```bash
sudo apt install tesseract-ocr-eng   # English
sudo apt install tesseract-ocr-fas   # Farsi or Persian
sudo apt install tesseract-ocr-ara   # Arabic
sudo apt install tesseract-ocr-spa   # Spanish
sudo apt install tesseract-ocr-fra   # French
```

### Text Extraction Commands
```bash
grim -g "$(slurp)" - | tesseract stdin stdout > ocr_result.txt
```

### Copy to Clipboard
```bash
grim -g "$(slurp)" - | tesseract stdin stdout | wl-copy
```

### English Text Recognition
Use the `-l` flag to specify the language. Replace `eng` with any installed Tesseract language code.
```bash
grim -g "$(slurp)" - | tesseract stdin stdout -l eng
```

### Translation Command
This command captures a screenshot, performs OCR on it, stores the recognized text inside the variable temp_var and then opens Google Translate by adding that text to the translation URL. In other words, it extracts the text, saves it temporarily and automatically sends it to Google Translate in the browser.
```bash
temp_var=$(grim -g "$(slurp)" - | tesseract stdin stdout) && firefox "https://translate.google.com/?sl=en&tl=fa&text=$temp_var&op=translate"
```

---

## GNOME Approach (X11/Wayland Compatible)

### Installation
```bash
sudo apt update
sudo apt install tesseract-ocr gnome-screenshot
sudo apt install wl-clipboard
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
gnome-screenshot -a -f /tmp/screenshot.png && firefox "https://translate.google.com/?sl=en&tl=fa&text=$(tesseract /tmp/screenshot.png stdout)&op=translate"
```

---

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

---

## Notes

- **Compatibility**: This may not work on every distro or window manager.
- **Wayland**: Uses `grim`/`slurp` for screenshots, supports direct stdin piping
- **GNOME**: Uses `gnome-screenshot`, requires temporary files but works on both X11 and Wayland
- **Language codes**: `eng` (English), `fas` (Farsi/Persian), `ara` (Arabic), `spa` (Spanish), etc.
- The translation command opens Google Translate with English to Farsi translation



