

#### Remove prefix
uses substitution to remove the start of the string
```bash
initial_string="prefix_something"
prefix="prefix"
result="${initial_string#$prefix}"
```


#### Remove suffix
uses substitution to remove the end of the string
```bash
initial_string="something_suffix"
suffix="suffix"
result="${initial_string%$suffix}"
```

#### TR command for string manipulation
https://www.cyberciti.biz/faq/linux-unix-shell-programming-converting-lowercase-uppercase/

##### convert to lower case
`echo "Data to convert" | tr '[:upper:]' '[:lower:]'`

or to convert an entire file

`tr '[:upper:]' '[:lower:]' < input_file.txt > output_file.txt `
