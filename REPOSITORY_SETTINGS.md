# Repository Settings Guide

This document explains how to ensure your GitHub repository allows public issue creation.

## GitHub Issues Are Public by Default

Good news! **GitHub issues are public by default** on public repositories. This means:

- ✅ Anyone can view issues
- ✅ Anyone with a GitHub account can create issues
- ✅ Anyone can comment on existing issues

## Verifying Issue Settings

To confirm that issues are enabled for your repository:

1. Go to your repository on GitHub
2. Click on **Settings** tab
3. Scroll down to the **Features** section
4. Ensure the **Issues** checkbox is **checked** ✓

If the Issues checkbox is checked, then:
- Issues are enabled
- Anyone can create them (no special permissions needed)
- The issue templates in `.github/ISSUE_TEMPLATE/` will be available

## What This PR Provides

This pull request adds:

1. **README.md** - Documentation about the repository and how to contribute
2. **CONTRIBUTING.md** - Detailed guide for contributors explaining that issues are public
3. **Issue Templates** - Pre-formatted templates for:
   - Recipe suggestions
   - Recipe improvements
   - Bug reports
   - General questions
4. **This guide** - Instructions for repository owners

## Making Issues More Accessible

To encourage public participation:

1. **Enable Issues** (as described above)
2. **Use the provided templates** - They guide contributors through the submission process
3. **Add topic tags** - Go to Settings and add relevant topics like `cookbook`, `recipes`, `community`
4. **Pin important issues** - Pin a "Welcome" or "How to Contribute" issue at the top
5. **Respond to issues** - Engage with contributors to build community

## Repository Visibility

If your repository is:
- **Public** ✅ - Issues are accessible to everyone (default for most repositories)
- **Private** ❌ - Only collaborators can see/create issues

To check repository visibility:
1. Go to **Settings** > **General**
2. Look at the **Danger Zone** section at the bottom
3. It will show "Change repository visibility"

## No Additional Configuration Needed

Since GitHub issues are public by default on public repositories, **no additional configuration is needed**. The templates and documentation provided in this PR will help guide contributors in creating well-formatted issues.

## Summary

✅ **Issues are already public** if your repository is public and Issues are enabled in Settings  
✅ **Anyone can contribute** by creating issues  
✅ **This PR provides templates** to make contributing easier  
✅ **No code changes needed** - this is purely documentation and templates

The community can now easily contribute recipes and improvements! 🍳
