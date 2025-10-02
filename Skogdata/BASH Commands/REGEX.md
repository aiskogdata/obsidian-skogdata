`^` tells regex to look at the start of the string
`$` says look for end of string
`(?=.)` is called a positive look ahead and matches anywhere in the string regardless or order or position
`?` following any particular symbol tells the expression this is optional
`/i` says 'be case insensitive when parsing string'
`+` says 'one or more of preceeding pattern'

### Match strings that are 2 to 5 characters long that are preceded by a space and followed by a space or a period.
```regex
	\s([A-z]{2,5})\w(\.|\s)
```

### Username validation
```
/^[a-zA-Z0-9_-]{3,16}$/
```

### Password Validation  
```
/^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[!@#$^&*()_-]).{8,18}$/
```
Match any string with at least 1 digit, 1 lower case, 1 upper case and 1 of the following `!@#$%^&*()_-` with lengths between 8 and 18

### Hex value
```
/^#?([a-f0-9]{6}|[a-f0-9]{3})$/i
```

"#" followed by any combo of letters between "a" and "f" or numbers with a length of either 6 or 3

### Email
```
/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,63})$/
```
Looks between start and end of string for any combination of lower and upper case letters, numbers, periods, hyphens, and underscores, followed by an @, followed by any lowercase letters, number, underscores, dots or hyphens, followed by a dot, and finally followed by 2-63 characters

grep alternative `$ grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" filename.txt`

### URL
```
/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
```
optionally look for `https://` at the beginning
any number letters, digits, dots and hyphens, followed by a dot, followed by 2 to 6 letters and dots, followed by any number or strings separated by `/` or `.`

### IP Address
```
/^(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/
```
Dont understand, just useful
but it looks for any combo of 1 to 3 numbers separated by a dot, repeating 4 times, with the total of each number not exceeding 255

### Match any HTML tag
```
/^<([a-z\d]+)([^<]+)*(?:>(.*)<\/\1>|\s*\/>)$/
```
Dont understand, just useful