```bash

echo "This is a line of text" | tr 'a' 'A'

echo "This is a line of text" | tr 'aeio' 'AEIO'

echo "This is a line of text" | tr -d 'aeio ' # delete charcter

echo "Thiis iis aaa liinee   oof text" | tr -s 'aeio ' # squeezed characters

echo "Thiis iis aaa liinee   oof text" | tr -s '[:lower:]' '[:upper:]' # squeezed characters and uppercase

echo "This is a line of text" | tr -cd 'aeio ' # delete every complement charcter -> i i a ie o e⏎

head 3 /dev/urandom | tr -cd  '[:print:]' # delete unprintable characters

echo "This is a strong password -1234" | tr -cd '[:digit:]' # -> 1234

```