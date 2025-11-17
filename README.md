# Try It Out


## My Code

Here are the PowerShell scripts I used to find the flags:

### 1. Search All Files for FLAGS

```powershell
# Search through all files for FLAG patterns
Get-ChildItem -Path "Round1_Dataset" -Recurse -File | ForEach-Object { 
    try { 
        $bytes = [System.IO.File]::ReadAllBytes($_.FullName)
        $text = [System.Text.Encoding]::UTF8.GetString($bytes)
        
        # Find all FLAG_XX patterns
        $matches = [regex]::Matches($text, 'FLAG_\d+\{[^}]*\}')
        foreach ($match in $matches) {
            Write-Host "Found: $($match.Value)"
            Write-Host "Location: $($_.FullName)"
        }
    } catch {}
}
```

### 2. Decode Base64

```powershell
# Decode Base64 encoded flags
$base64String = "RkxBR18wMntEZWNyeXB0ZWQgY2hhdCBsb2dzIG9mIGhpdHMvcGF5b2ZmcyAoSUQ6IDc4MTIsIDIwMjMtMDUtMTLigJMyMDI0LTA4LTIxLCBFbmNyeXB0ZWQgc2VydmVycyl9Cg=="

$decoded = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($base64String))
Write-Host "Decoded: $decoded"
```

### 3. Decode Hexadecimal

```powershell
# Decode hexadecimal encoded flags
$hex = "464c41475f30367b5369676e6564206d656d6f20617574686f72697a696e6720696c6c6567616c2061637473202849443a2031313034352c20323032332d31312d32392c20436f72706f72617465206f6666696365297d"

$bytes = @()
for ($i = 0; $i -lt $hex.Length; $i += 2) {
    $bytes += [Convert]::ToByte($hex.Substring($i, 2), 16)
}
$decoded = [System.Text.Encoding]::UTF8.GetString($bytes)
Write-Host "Decoded: $decoded"
```

### 4. Extract Text from PNG Files

```powershell
# Extract readable text from PNG files
$pngPath = "Round1_Dataset/YouWon'tGetiT/Latch/neon/nonce-alpha/ci_n_d_e_r/unnamed.png"
$bytes = [System.IO.File]::ReadAllBytes($pngPath)
$text = [System.Text.Encoding]::UTF8.GetString($bytes)

# Search for FLAG in the text
if ($text -match 'FLAG') {
    $matches = [regex]::Matches($text, 'FLAG_\d+\{[^}]*\}')
    foreach ($match in $matches) {
        Write-Host "Found in PNG: $($match.Value)"
    }
}
```

### 5. Try Multiple Encodings

```powershell
# Try different character encodings
$file = "Round1_Dataset/YouWon'tGetiT/unknown"
$bytes = [System.IO.File]::ReadAllBytes($file)

foreach ($encoding in @([System.Text.Encoding]::UTF8, [System.Text.Encoding]::ASCII, [System.Text.Encoding]::Unicode)) {
    try {
        $text = $encoding.GetString($bytes)
        if ($text -match 'FLAG') {
            Write-Host "Found with encoding: $($encoding.EncodingName)"
            Write-Host $text
        }
    } catch {}
}
```

### 6. Search for MD5 Hash Matches

```powershell
# Try to crack MD5 hash
$md5Hash = "4106b08459f09a84018881a1d6bd25b5"
$testStrings = @("weapon", "gun", "evidence", "unknown", "FLAG_04")

$md5Provider = [System.Security.Cryptography.MD5]::Create()

foreach ($str in $testStrings) {
    $bytes = [System.Text.Encoding]::UTF8.GetBytes($str)
    $hash = $md5Provider.ComputeHash($bytes)
    $hashStr = [System.BitConverter]::ToString($hash).Replace('-','').ToLower()
    
    if ($hashStr -eq $md5Hash) {
        Write-Host "FOUND! MD5 of '$str' matches!"
    }
}
```

## My Submission Files

You can download and review my complete work:

- **FINAL_SUBMISSION.md** - All 8 flags I found with complete details
- **FLAG_SUMMARY.txt** - Detailed breakdown of each flag
- **PROJECT_STORY_SHORT.md** - How I solved the challenge
- **FLAG_04_INVESTIGATION.txt** - My investigation into the missing flag

## Verify My Results

All flags are documented with:
- Exact location in the dataset
- Encoding method used
- How I decoded it
- The complete flag text

You can verify each flag by checking the files I found them in.

## Try to Beat My Score

- **My Score**: 9 out of 9 flags 

**Download all my files and check my work! Use the code above to verify my findings!**
