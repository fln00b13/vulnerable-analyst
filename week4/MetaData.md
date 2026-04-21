Tools Learned

exiftool
hexeditor
strings
binwalk
file

# 📊 Metadata & Forensics Tools Table

1. Exiftool
2024_Nitro_Default_3840x2400.jpg
https://exif.tools/
<img width="1914" height="1019" alt="Screenshot 2026-04-21 183033" src="https://github.com/user-attachments/assets/49bf08b0-0afd-437f-9a71-c17bf30c63ce" />
Extracts metadata such as comments, timestamps, and camera info; flag found in comment field.

2. Hexeditor
download.jpeg
https://hexed.it/
<img width="1917" height="1023" alt="Screenshot 2026-04-21 183240" src="https://github.com/user-attachments/assets/378d8f27-03d7-49dc-8689-06c4eb0693fa" />
Used to inspect file headers and detect file structure or hidden data.

3. Strings
Creepy_Dolphin.webp
https://www.dcode.fr/strings-extractor
<img width="1559" height="607" alt="Screenshot 2026-04-21 183415" src="https://github.com/user-attachments/assets/06dbeeba-fc87-43f0-b48f-aaa50d4f6758" />
Extracts readable text from binary files such as hidden messages or URLs.

4. Binwalk
browser-home-page-banner.jpg
binwalk browser-home-page-banner.jp
binwalk -e browser-home-page-banner.jp
cd _browser-home-page-banner.jp.extracted/
<img width="771" height="392" alt="image" src="https://github.com/user-attachments/assets/441c46f0-7c20-4f85-b3c0-375aaa6b8825" />
Detects embedded files hidden inside images (steganography/file embedding).


5. File
QR_C0d3.png
file QR_C0d3.png
<img width="313" height="61" alt="image" src="https://github.com/user-attachments/assets/ea5b9770-d4ad-467d-b9ea-ade76d1e07c7" />
Identifies real file type; detects mismatched extension (spoofed file).
