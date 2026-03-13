 ##  \ ==> dege toh space ki sath folder ka nam likh sakte hia 
  ex: mkdir backend\ spring boot

 ##  ==> git status    (most used karte raho)

===============================
Agar **Git me commit ho chuka hai aur aapko wapas previous state par jaana hai**, to situation ke hisaab se different commands use hoti hain. Yahan sabse common options diye hain 👇

---

## 1️⃣ Last commit ko undo karna (code changes keep karna)

Agar last commit hataana hai lekin code changes rehne dene hain:

```bash
git reset --soft HEAD~1
```

✅ Last commit remove ho jayega
✅ Changes staging area me rahenge

---

## 2️⃣ Last commit undo + changes unstage karna

Agar commit bhi hataana hai aur changes unstaged chahiye:

```bash
git reset --mixed HEAD~1
```

Ye default reset mode hota hai.

---

## 3️⃣ Last commit completely delete karna

Agar commit aur uske changes dono delete karne hain:

```bash
git reset --hard HEAD~1
```

⚠️ Warning: Changes permanently delete ho jayenge.

---

## 4️⃣ Agar commit push bhi ho gaya hai

Agar remote (GitHub/GitLab) par push ho chuka hai aur revert karna hai:

```bash
git revert HEAD
```

Ye **naya commit create karke previous commit ko reverse karta hai**.

---

✅ **Quick summary**

| Situation                     | Command                    |
| ----------------------------- | -------------------------- |
| Commit undo but keep changes  | `git reset --soft HEAD~1`  |
| Commit undo + unstage changes | `git reset --mixed HEAD~1` |
| Commit delete completely      | `git reset --hard HEAD~1`  |
| Pushed commit revert          | `git revert HEAD`          |

---


===========================
`git rm --cached` command ka use **file ko Git tracking se hataane ke liye** hota hai, lekin **file aapke local folder me delete nahi hoti**.

### 📌 Syntax

```bash
git rm --cached <file-name>
```

### 🔹 Example

```bash
git rm --cached config.json
```

Iska result:

* `config.json` **Git se remove ho jayega (untracked ho jayega)**
* Lekin **file aapke system me rahegi**.

---

### 🧠 Ye kab use hota hai?

#### 1️⃣ Agar galti se koi file commit ho gayi ho

Jaise:

* `.env`
* `node_modules`
* `config.json`
* secrets / passwords wali file

To aap use Git se remove kar sakte ho:

```bash
git rm --cached .env
```

Phir `.gitignore` me add kar do:

```
.env
```

---

#### 2️⃣ Agar poora folder untrack karna ho

```bash
git rm -r --cached node_modules
```

---

### 🔍 Difference samjho

| Command                | Kya karta hai                                      |
| ---------------------- | -------------------------------------------------- |
| `git rm file`          | File Git se bhi delete + system se bhi delete      |
| `git rm --cached file` | Sirf Git tracking se remove, file system me rahegi |

---

✅ **Typical workflow**

```bash
git rm --cached .env
echo ".env" >> .gitignore
git commit -m "remove env from tracking"
```

---

================================
## git log 

=========================
only conflict comes by ==> same file and same line 
=========================


