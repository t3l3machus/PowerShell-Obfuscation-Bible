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
 !$False
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
 - The result of a comparison:
 ```
 $x = (9999 -eq 9999) # Simple
 $x = ([math]::Round([math]::PI) -eq (4583 - 4580)) # Complex
 ```

 - Boolean type casting something you know will return a value Not equal to 0 (can also be a string, array, etc)
 ```
 [bool](Get-ChildItem -Path Env: | Where-Object {$_.Name -eq "username"})
 [bool]@(0x01BE)
 ```
[System.Data.AcceptRejectRule].Assembly.GlobalAssemblyCache
![System.Data.AcceptRejectRule].Assembly.GlobalAssemblyCache
i''e''x''


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
```

