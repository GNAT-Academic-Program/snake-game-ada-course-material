---
kind: unit

title: Fork the Course Repository

name: 1-snake
---

Let's set up your own copy of the course code! Forking allows you to save your progress and access reference code at any time.

## Why Fork?

**Personal Progress:** Commit and save your own implementations, even create your own game variants.

**Reference Access:** Revert to any lesson's working code using tagged snapshots from the original repository.

## Setting Up Your Fork

1. **Create a GitHub account** if you don't have one:
   - Visit [github.com](https://github.com) and sign up
   - Essential for any developer's toolkit

2. **Fork the repository**:
   - Open: https://github.com/GNAT-Academic-Program/snake-game-ada-course-code
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
   git clone https://github.com/<YOUR_GITHUB_USERNAME>/snake-game-ada-course-code.git
   ```

5. **Set up upstream and fetch lesson tags**:
   ```bash
   cd snake-game-ada-course-code
   git remote add upstream https://github.com/GNAT-Academic-Program/snake-game-ada-course-code.git
   git fetch upstream --tags
   git checkout -B lesson-02-work refs/tags/lesson-02-start
   ```



✅ **Expected result:** You now have your own fork with access to all lesson snapshots and can track your progress!
