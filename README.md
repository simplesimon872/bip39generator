## Verify this file

SHA-256: EB49F7FF174FF34640F94A6045A81C4926E373D61A4B3EDE1A2D71976B9906F6

Anyone can verify the hosted file hasn't been modified:

**Windows (PowerShell):**
curl https://github.io/simplesimon872/bip39-generator/ -o index.html
Get-FileHash index.html -Algorithm SHA256

**Mac/Linux:**
curl -O https://github.io/simplesimon872/bip39-generator/ 
sha256sum index.html

Compare the output to the hash above. If they match, the file is unmodified.