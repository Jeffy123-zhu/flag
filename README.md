I used these six PowerShell scripts to search, decode, and find the hidden messages.

1. Search All Files for FLAGS
This program looks through every single file in the Round1_Dataset folder. It tries to find anything that looks like a flag, following the pattern FLAG_XX{...}.

PowerShell

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
2. Decode Base64
Some flags were hidden using Base64 encoding. This script takes the coded string and turns it back into normal text.

PowerShell

# Decode Base64 encoded flags
$base64String = "RkxBR18wMntEZWNyeXB0ZWQgY2hhdCBsb2dzIG9mIGhpdHMvcGF5b2ZmcyAoSUQ6IDc4MTIsIDIwMjMtMDUtMTLigJMyMDI0LTA4LTIxLCBFbmNyeXB0ZWQgc2VydmVycyl9Cg=="

$decoded = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($base64String))
Write-Host "Decoded: $decoded"
3. Decode Hexadecimal
Another flag was hidden as a long line of hexadecimal numbers (base 16). This script converts those numbers into readable letters and symbols.

PowerShell

# Decode hexadecimal encoded flags
$hex = "464c41475f30367b5369676e6564206d656d6f20617574686f72697a696e6720696c6c6567616c2061637473202849443a2031313034352c20323032332d31312d32392c20436f72706f72617465206f6666696365297d"

$bytes = @()
for ($i = 0; $i -lt $hex.Length; $i += 2) {
    $bytes += [Convert]::ToByte($hex.Substring($i, 2), 16)
}
$decoded = [System.Text.Encoding]::UTF8.GetString($bytes)
Write-Host "Decoded: $decoded"
4. Extract Text from PNG Files
I checked if any flags were hidden inside the invisible parts of PNG image files. This program reads the file's raw data and searches for the flag pattern.

PowerShell

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
5. Try Multiple Encodings
For one mysterious file, I had to try different ways (encodings like UTF8, ASCII, and Unicode) to read the text correctly.

PowerShell

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
6. Search for MD5 Hash Matches
I found an MD5 hash (a unique digital fingerprint) and tried to guess the original word that created it.

PowerShell

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
📂 My Submission Files
You can download and check all my finished work:

FINAL_SUBMISSION.md - A list of all 9 flags I found with every detail.

FLAG_SUMMARY.txt - A quick summary of each flag and where it was located.

PROJECT_STORY_SHORT.md - A short story about how I solved the whole challenge.

FLAG_04_INVESTIGATION.txt - My notes on looking for the one flag that was difficult to find.

✅ Verify My Results
I wrote down everything for each flag:

The exact place in the dataset.

The encoding method that was used.

How I decoded it.

The complete flag text.

You can use the files and the code above to check if all my findings are correct.

🏆 Try to Beat My Score
I found every flag!

My Score: 9 out of 9 flags

Go ahead! Download my files and check my work! Use the code above to see if you can find them too!

Would you like me to explain any of the decoding processes in simpler terms?
