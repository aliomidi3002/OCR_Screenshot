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
Use the `-l` flag to specify the language. Replace `eng` with any installed Tesseract language code. You can also combine multiple languages by joining their codes with a plus sign such as `eng+fas` for English and Farsi recognition.
```bash
grim -g "$(slurp)" - | tesseract stdin stdout -l eng
```

### Translation Command
This command captures a screenshot, performs OCR on it, stores the recognized text inside the variable temp_var and then opens Google Translate by adding that text to the translation URL. In other words, it extracts the text, saves it temporarily and automatically sends it to Google Translate in the browser.
```bash
temp_var=$(grim -g "$(slurp)" - | tesseract stdin stdout) && firefox "https://translate.google.com/?sl=en&tl=fa&text=$temp_var&op=translate"
```

---

## GNOME-X11 Approach

### Installation
```bash
sudo apt update
sudo apt install tesseract-ocr gnome-screenshot
sudo apt install wl-clipboard
```

### Text Extraction Commands
```bash
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout | wl-copy
```

### English Text Recognition
Use the `-l` flag to specify the language. Replace `eng` with any installed Tesseract language code. You can also combine multiple languages by joining their codes with a plus sign such as `eng+fas` for English and Farsi recognition
```bash
gnome-screenshot -a -f /tmp/screenshot.png && tesseract /tmp/screenshot.png stdout -l eng | wl-copy
```

### Translation Command
This command captures a screenshot, performs OCR on it, stores the recognized text inside the variable temp_var and then opens Google Translate by adding that text to the translation URL. In other words, it extracts the text, saves it temporarily and automatically sends it to Google Translate in the browse
```bash
gnome-screenshot -a -f /tmp/screenshot.png && firefox "https://translate.google.com/?sl=en&tl=fa&text=$(tesseract /tmp/screenshot.png stdout)&op=translate"
```



## License

This project is licensed under the MIT License.  
See the [LICENSE](LICENSE) file for more details.
