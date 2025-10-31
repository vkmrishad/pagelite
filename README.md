# Pagelite
A lightweight static blog powered by SQLite and SQL.js — runs entirely on GitHub Pages.

---

## Pagelite - Static Blog Powered by SQL.js (WASM)

Pagelite is a lightweight static blog system that runs entirely in the browser using SQL.js (WebAssembly).  
It uses a local SQLite database (`blog.db`) to store posts and loads them dynamically — no backend is required.

---

## 📁 Project Structure
```text
pagelite/
├── index.html          -> Lists all blog posts (titles only)
├── post.html           -> Displays a full blog post
├── create.html         -> Page to create new posts (client-side editor)
├── blog.db             -> SQLite database (posts table)
└── assets/
    ├── js/
    │   ├── sql-wasm/1.13.0/
    │   │   ├── sql-wasm.min.js
    │   │   └── sql-wasm.wasm
    │   └── quill/2.0.2/
    │       └── quill.js
    ├── css/
    │   └── quill/2.0.2/
    │       └── quill.snow.css
```

---

## ⚙️ How It Works

### 1. index.html
- Loads `blog.db` using SQL.js (SQLite compiled to WASM).  
- Queries and lists all posts from the `blogs` table:
  ```sql
  SELECT id, title FROM blogs ORDER BY id DESC;
  ```
- Clicking a post opens:  
  `post.html?id=<id>`

### 2. post.html
- Reads the post `id` from the URL query string.  
- Loads `blog.db` again and displays the selected post:
  ```sql
  SELECT title, content FROM blogs WHERE id = ?;
  ```

### 3. create.html
- Provides a rich text HTML editor (powered by **Quill.js 2.0.2**).  
- When you click **"Save Post & Export DB"**:
  - Inserts the new post into the SQLite database (in memory).
  - Exports the updated database.
  - Automatically downloads the new `blog.db` file.

---

## 💾 How to Update Your Blog

Browsers cannot directly write files to your server.  
After creating or editing posts, you must manually replace the old database file.

**Steps:**
1. Open `create.html` in your browser.  
2. Write your new post and click **"Save Post & Export DB"**.  
3. A file named `blog.db` will be downloaded.  
4. Replace the existing `blog.db` file in your project folder (or `/assets/data/` if used there).  
5. Refresh `index.html` to see your updated posts.

---

## 🧩 SQLite Table Structure
```sql
CREATE TABLE blogs (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  title TEXT NOT NULL,
  content TEXT
);
```

---

## 🧠 Requirements
- You must serve the site using a local or online web server  
  (fetch does not work with `file://` URLs).

**Examples:**
```bash
npx http-server .
# or
python3 -m http.server
```

Then open:
```
http://localhost:8080/index.html
```

---

## 🚀 Optional Enhancements
- Store the database in IndexedDB for persistent local saving.
- Add a backend (Node.js, Python, PHP) to save posts permanently.
- Add timestamps, tags, or categories to the `blogs` table.
- Allow Markdown input instead of HTML for posts.
