# Day 11 – Replace String in File using sed

## Objective
Replace all occurrences of the word **String** with **Maritime** in the file `/root/nautilus.xml`.

## Command

```bash
sudo sed -i 's/String/Maritime/g' /root/nautilus.xml
```

## Explanation

- `sed` is used for stream editing.
- `-i` edits the file in place.
- `s/String/Maritime/g` replaces all occurrences of "String" with "Maritime".

## Result
All instances of **String** were successfully replaced with **Maritime** in the XML file.
