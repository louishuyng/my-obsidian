- sed searching by string not word
```bash
sed 's/o/O/' <.Xresources >sed-test

echo "The vim file manager is dired" | sed 's/red/green/'
echo "The vim file manager is dired red" | sed 's/ red/ green/'

sed -i 's/o/O/g' filename # write the changes to file

sed '/line_pattern/s/find/replace/' filename # Replace only on lines mathching the line pattern

sed '/line_pattern/d' filename # Delete the line matching the line pattern

cat /etc/shells | sed -e 's/usr/u/g' -e 's/bin/b/g' # Multiple

sed -n '/usr/p' test.bash # print when match pattern usr

sed -i 's/ *$//' test.sh # remove empty space at the end of line

sed -i 's/[[:space:]]*$//' test.sh # remove tab at the end of line

cat test.sh | sed '/^$/d' # remove empty line

sed 's/[a-z]/\U&/g' test.sh # captilize
sed 's/[a-z]/\L&/g' test.sh # lowercase

sed 10q .bashrc # get first 10 lines

```