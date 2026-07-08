# Developer Notes for FootClothes Apps

Practical gotchas for anyone building or maintaining the internal apps in this
repo. Add to this file as new issues are discovered.

## File encoding: source files must be UTF-8 (not UTF-16)

### Symptom
Builds (Vite/TypeScript/Node) fail with errors like:

- `error TS1127: Invalid character.` on nearly every column of a file
- `EJSONPARSE` when running `npm install`
- `ERR_INVALID_TYPESCRIPT_SYNTAX` when running Node scripts

Opening the file in an editor looks fine, but a hex dump shows a byte pattern
like `69 00 6D 00 70 00 ...` (an ASCII char followed by a `00` byte). That is
UTF-16LE. TypeScript, Vite, and Node all expect UTF-8, so the interleaved null
bytes read as invalid characters.

### Cause
This is **not** a Google Drive issue and not a runtime issue - the shipped/built
app is unaffected. It comes from the file-writing path on Windows. Windows
PowerShell 5.1 defaults many write operations (`>`, `Out-File`, `Set-Content`,
and some editor/agent write tools) to "Unicode", which is UTF-16LE. Files
created or rewritten through those paths land as UTF-16 instead of UTF-8.

### Fix
Convert the affected files to UTF-8 (no BOM). PowerShell one-liner that detects
UTF-16 (a null byte in the first 400 bytes) and rewrites only those files:

```powershell
Get-ChildItem -Recurse -Include *.ts,*.tsx,*.js,*.json,*.css,*.html src | ForEach-Object {
  $bytes = [System.IO.File]::ReadAllBytes($_.FullName)
  $n = [Math]::Min(400, $bytes.Length)
  $hasNull = $false
  for ($i = 0; $i -lt $n; $i++) { if ($bytes[$i] -eq 0) { $hasNull = $true; break } }
  if ($hasNull) {
    $text = [System.Text.Encoding]::Unicode.GetString($bytes)
    $text = $text.TrimStart([char]0xFEFF)
    [System.IO.File]::WriteAllText($_.FullName, $text, (New-Object System.Text.UTF8Encoding($false)))
    Write-Host "fixed $($_.Name)"
  }
}
```

Important: read the bytes with `[System.Text.Encoding]::Unicode` (UTF-16LE). Do
NOT read with `ReadAllText` default first - that misreads a BOM-less UTF-16 file
as UTF-8 and preserves the null bytes, so the "fix" does nothing.

### Prevention
- Prefer editors/tools configured to write UTF-8.
- Consider an `.editorconfig` with `charset = utf-8` and a `prebuild` npm script
  that runs the normalizer above so builds self-heal.
- In PowerShell 7+, default encoding is UTF-8, which avoids the problem.
