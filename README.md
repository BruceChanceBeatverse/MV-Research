# AGNO-MV Research Repository

This is an independent Git repository containing comprehensive research documentation for the AGNO-MV-2.0-Performance project.

## Repository Structure

```
Research/
├── Gemini/                      # Gemini 2.5 Pro system prompting guides
├── Google censorship/           # Content moderation guidelines
├── Heygen Avatar IV/            # HeyGen Avatar IV API and prompting guides
├── Music video Research/        # Music video production guides
├── Nano banana research/        # Gemini 2.5 Flash Image (Nano-Banana) guides
├── Omnihuman 1.5/              # OmniHuman 1.5 documentation
│   ├── OmniHuman-1.5-Research-Summary.md
│   ├── OmniHuman-1.5-Prompt-Guide.md
│   ├── OmniHuman-1.5-User-Manual.md
│   └── Original/               # Original .docx files
├── Performance Music Videos/    # Performance-style music video guides
├── Pixverse research/          # PixVerse V5 prompting guides
└── Platform Comparison - HeyGen vs OmniHuman.md
```

## Current Setup

This folder is currently set up as a **separate Git repository** within the main AGNO-MV-2.0-Performance project. This allows for:

- ✅ **Independent version control** - Research has its own commit history
- ✅ **Separate branching** - Can create research-specific branches
- ✅ **Cross-project sharing** - Can be cloned/used in other projects
- ✅ **Clean separation** - Main project doesn't track research changes

## Working with This Repository

### Making Changes

```bash
# Navigate to Research folder
cd Research/

# Check status
git status

# Add and commit changes
git add .
git commit -m "Your commit message"

# View history
git log --oneline
```

### Current Status

- **Main repo:** AGNO-MV-2.0-Performance ignores Research/ folder (via .gitignore)
- **Research repo:** Independent Git repository with its own history
- **Remote:** Not yet configured (local only)

## Next Steps: Setting Up as Git Submodule

If you want to convert this to a proper Git submodule (recommended for sharing across projects), follow these steps:

### 1. Create Remote Repository

Create a new repository on GitHub, GitLab, or your preferred Git hosting:

```bash
# Example repository name
AGNO-MV-2.0-Research
```

### 2. Push Research Repository to Remote

```bash
cd /Users/brucechen/Desktop/AGNO-MV-2.0-Performance/Research

# Add remote (replace with your actual remote URL)
git remote add origin git@github.com:yourusername/AGNO-MV-2.0-Research.git

# Push to remote
git push -u origin main
```

### 3. Convert to Submodule in Main Repo

```bash
cd /Users/brucechen/Desktop/AGNO-MV-2.0-Performance

# Remove Research/ from .gitignore
# Edit .gitignore and remove the "Research/" line

# Remove the Research folder (backup first if needed!)
rm -rf Research/

# Add as submodule (replace with your actual remote URL)
git submodule add git@github.com:yourusername/AGNO-MV-2.0-Research.git Research

# Commit the submodule addition
git add .gitmodules Research
git commit -m "Add Research as submodule"

# Push to remote
git push
```

### 4. Working with Submodule

Once set up as a submodule, collaborators need to initialize it:

```bash
# Clone main repo with submodules
git clone --recurse-submodules <main-repo-url>

# Or if already cloned
git submodule init
git submodule update
```

To update submodule:

```bash
cd Research/
git pull origin main
cd ..
git add Research
git commit -m "Update Research submodule"
git push
```

## Benefits of Submodule Setup

Once converted to a submodule:

1. **Versioning**: Main repo tracks specific commits of Research repo
2. **Collaboration**: Multiple people can work on research independently
3. **Reusability**: Research repo can be included in multiple projects
4. **Clean history**: Research changes don't clutter main repo history

## Alternative: Keep as Separate Repo

You can also keep the current setup (separate repo, no submodule) if you:

- Only work on this project locally
- Don't need to share research with other projects
- Prefer simpler Git workflow

This is a valid approach and requires no additional setup!

## Research Documentation Highlights

### OmniHuman 1.5
- **Comprehensive research summary** for audio+image to video generation
- **Multi-person singing/rapping** support with character selection
- **Prompt engineering guide** with best practices
- **API integration** documentation

### HeyGen Avatar IV
- **Music video generation** guide
- **API code examples** and integration patterns
- **Prompting cheat sheet** for optimal results

### Gemini 2.5 & Nano-Banana
- **System prompting guides** for Gemini 2.5 Pro
- **Image generation** with Gemini 2.5 Flash (Nano-Banana)
- **Prompt engineering** best practices

### PixVerse V5
- **Image-to-video** generation guides
- **Start-end frame** strategies
- **Character performance** and camera control techniques

---

**Last Updated:** 2025-11-12
**Repository Status:** Independent Git repository (local only, not yet configured as submodule)
