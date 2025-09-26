---
kind: unit

title: Fork the Course Repository

name: 1-snake
---

Let's set up your own copy of the course code! Forking allows you to save your progress and access reference code at any time.

## Why Fork?

**Personal Progress:** Commit and save your own implementations, even create your own game variants.

**Reference Access:** Quickly return to any lesson’s code using saved snapshots in the original repository.

## Setting Up Your Fork

1. **Create a GitHub account** if you don't have one:
   - Visit [github.com](https://github.com) and sign up
   - A GitHub account is essential for any developer

2. **Fork the repository**:
   - Open: https://github.com/GNAT-Academic-Program/snake-tutorial
   - Click the **Fork** button (top-right corner)
   - Click **Create fork** on the confirmation screen

3. **Open a terminal** here in the IDE:
   - Drag up from the bottom edge
   - Or use menu → **Terminal → New Terminal**

::remark-box
---
kind: info
---
🤯 **Heads Up!** Always use `Ctrl+Shift+V` to paste in the terminal. This is different from regular copy-paste in the IDE.
::

4. **Clone your fork** (replace `<YOUR_GITHUB_USERNAME>` with your actual GitHub username):
   ```bash
   git clone https://github.com/<YOUR_GITHUB_USERNAME>/snake-tutorial.git
   ```

5. **Set up upstream and fetch lesson branches**:
   ```bash
   cd ~/sandbox/snake-tutorial
   git remote add upstream https://github.com/GNAT-Academic-Program/snake-tutorial.git
   git fetch upstream
   git checkout -b lesson-02-work upstream/lesson-02-start
   git push -u origin lesson-02-work
   ```

✅ **Expected result:** You now have your own fork with access to all lesson snapshots and can track your progress!

