# Vector - File Finder Project 🚀

## 🎯 What I'm Building
A **hierarchy-aware fuzzy file finder** that lets me type space-separated path fragments to instantly locate files in deep directory structures.

**Core idea**: `v repository hello main` → `projects/repository/hello-git/hello/main.rs`

*Inspired by zoxide's intelligence + fzf's interactivity, but specifically for file discovery.*

---

## 🧠 My Vision & Use Cases

### Daily Pain Points I'm Solving:
- Navigating `src/components/dashboard/widgets/charts/LineChart.jsx` manually
- Finding config files buried 5+ levels deep
- Locating test files that mirror source structure
- Opening documentation scattered across project folders

### My Workflow Goals:
```bash
# Instead of: cd projects && cd my-app && cd src && cd components && ls
v my-app components user profile
# → src/components/user/UserProfile.jsx

# Instead of: find . -name "*config*" | grep webpack
v webpack config  
# → build/webpack.config.js
```

---

## 🚨 CRITICAL: Multiple Matches Problem & Solutions

### The Challenge I Need to Solve
When `v test user` matches 15+ files, what happens? This is make-or-break for user experience.

### My Tiered Strategy

#### Tier 1: Single Match (0-1 results)
- **0 matches**: Show "No matches found. Try: [suggestions]"
- **1 match**: Output path directly, no interaction needed

#### Tier 2: Few Matches (2-5 results)  
```bash
v test user
1. tests/user.test.js                    [last modified: 2h ago]
2. src/models/user.js                   [accessed: yesterday] 
3. docs/user-guide.md                   [size: 2.1kb]
Select [1-3] or press Enter for #1: _
```
**Implementation notes:**
- Default to #1 (best match) on Enter
- Show context clues (recency, size, location)
- Number selection for speed

#### Tier 3: Many Matches (6+ results)
Launch fzf-style interactive picker:
- Live filtering as I type more
- File preview in split pane
- Syntax highlighting for code files
- Recent files marked with ★

### My Ranking Algorithm (MVP → Advanced)

**MVP Ranking (simple but effective):**
1. Exact word matches > fuzzy matches
2. Fewer directory levels = higher rank  
3. Recently modified files boost score

**Advanced Ranking (future):**
1. **Frecency**: Files I access often + recently (zoxide-style)
2. **Context awareness**: Current git branch, working directory
3. **Pattern learning**: If I always pick `*.test.js` over `*.js`, learn this
4. **File type intelligence**: `.rs` files if I'm in a Rust project

---

### Key Libraries & Tools
- **Rust**: `clap` (CLI), `walkdir` (traversal), `fuzzy-matcher` (matching)
- **Shell integration**: Fish/Zsh functions for keybindings
- **Fallback to fzf**: If installed, use for complex selection

### Potential Gotchas I Need to Handle
1. **Performance**: Large codebases (100k+ files) - need indexing?
2. **Symlinks**: Follow or ignore? Could create infinite loops
3. **Hidden files**: Include `.env`, `.gitignore` or skip?
---

