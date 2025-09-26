---
kind: unit

title: Get Latest Code

name: 1-snake
---

🚀 Beginning of Lesson 4 — Two Options

Each lesson starts with a new machine, you need to pull the latest code first, then either resume your own progress or start from the official snapshot.

**Clone your fork (replace `<YOUR_GITHUB_USERNAME>`):**

```
git clone https://github.com/<YOUR_GITHUB_USERNAME>/snake-tutorial.git
cd ~/sandbox/snake-tutorial
```

### 🅰️ Option A — Resume from Your Fork (Recommended)

Check out your saved progress to a branch:

```
git checkout -b lesson-04-work origin/lesson-03-work
```

✅ You're now ready to start Lesson 4 from your own progress.

### 🅱️ Option B — Start from Official Course Snapshot

Check out and push to your origin the starting point for Lesson 4 :

```
git remote add upstream https://github.com/GNAT-Academic-Program/snake-tutorial.git
git fetch upstream
git checkout -b lesson-04-work upstream/lesson-04-start
git push -u origin lesson-04-work
```

✅ You're now starting Lesson 4 from the clean official reference code.
