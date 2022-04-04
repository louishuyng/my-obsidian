```bash
echo "This is a line of text!" | cut -c 1-10 # cut by character 

cut -c 11- ~/.bashrc # cut 11 to end line

echo "This is a line of text!" | cut -d ' ' -f5 # cut by dilimiter

cut -d ':' -f2 /etc/passwd

```