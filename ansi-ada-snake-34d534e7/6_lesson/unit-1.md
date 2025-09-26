---
kind: unit

title: Get Latest Code

name: 1-snake
---


🚀 Beginning of Lesson 6 — Two Options

Each lesson starts with a new machine, you need to pull the latest code first, then either resume your own progress or start from the official snapshot.

**Clone your fork (replace `<YOUR_GITHUB_USERNAME>`):**

```
git clone https://github.com/<YOUR_GITHUB_USERNAME>/snake-tutorial.git
cd ~/sandbox/snake-tutorial
```

### 🅰️ Option A — Resume from Your Fork (Recommended)

Check out your saved progress to a branch:

```
git checkout -b lesson-06-work origin/lesson-05-work
```

✅ You're now ready to start Lesson 6 from your own progress.

### 🅱️ Option B — Start from Official Course Snapshot

Check out and push to your origin the starting point for Lesson 6 :

```
git remote add upstream https://github.com/GNAT-Academic-Program/snake-tutorial.git
git fetch upstream
git checkout -b lesson-06-work upstream/lesson-06-start
git push -u origin lesson-06-work
```

✅ You're now starting Lesson 6 from the clean official reference code.
