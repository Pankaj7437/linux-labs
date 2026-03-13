# DevOps Task: Modify BSD.txt on App Server 2

## Task Description
On Nautilus **App Server 2**, the file `/home/BSD.txt` needed to be processed according to the following requirements:

1. Delete all lines containing the word **following** and save the output to `/home/BSD_DELETE.txt`.
2. Replace all occurrences of the exact word **or** with **them** and save the output to `/home/BSD_REPLACE.txt`.

Note: Only the exact word **or** should be replaced. Words containing `or` such as `word` or `origin` must remain unchanged.

---

## Server Information

Server: App Server 2  
Host: stapp02  
User: steve  
Source File: /home/BSD.txt  

---

## Step 1: Connect to the Server

ssh steve@stapp02

---

## Step 2: Remove Lines Containing "following"

Command used:

grep -v "following" /home/BSD.txt > /home/BSD_DELETE.txt

Explanation:

- `grep` searches for text patterns
- `-v` excludes lines containing the pattern
- Output is redirected to `/home/BSD_DELETE.txt`

---

## Step 3: Replace the Word "or" with "them"

Command used:

sed 's/\bor\b/them/g' /home/BSD.txt > /home/BSD_REPLACE.txt

Explanation:

- `sed` is used for text substitution
- `\b` ensures word boundary matching
- This ensures only the exact word `or` is replaced

---

## Verification

cat /home/BSD_DELETE.txt  
cat /home/BSD_REPLACE.txt

---

## Output Files

/home/BSD_DELETE.txt → File without lines containing "following"  
/home/BSD_REPLACE.txt → File where the word "or" is replaced with "them"

---

## Tools Used

- SSH
- grep
- sed

---

## Conclusion

The required text processing was completed successfully using standard Linux utilities.  
The original file `/home/BSD.txt` remained unchanged while the processed results were stored in separate files.
