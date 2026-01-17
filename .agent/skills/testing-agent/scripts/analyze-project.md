# Project Analysis Patterns

Use these patterns when analyzing a project to identify testable features.

## Discovery Strategy

### 1. Page Discovery

Scan for routes/pages:

```
# Next.js/React
Look in: src/app/**/page.tsx, pages/**/*.tsx

# Plain HTML
Look for: **/*.html

# Vue
Look in: src/views/**/*.vue, src/pages/**/*.vue
```

For each page found, note:
- Route path
- Page purpose (from filename/directory)
- Whether it requires authentication
- Key components on the page

### 2. Component Discovery

Identify interactive components:

```
# Look for form elements
grep -r "onSubmit|onClick|onChange" --include="*.tsx" --include="*.jsx"

# Look for modals/dialogs
grep -r "Modal|Dialog|Drawer|Sheet" --include="*.tsx"

# Look for navigation
grep -r "Link|router|navigate" --include="*.tsx"
```

### 3. API Discovery

Find backend endpoints:

```
# Next.js API routes
Look in: src/app/api/**/route.ts

# Express routes
Look for: router.get|router.post|router.put|router.delete

# Fetch calls
grep -r "fetch\(|axios\." --include="*.ts" --include="*.tsx"
```

### 4. Flow Discovery

Read `.planning/` documents to understand:
- User journeys documented in PROJECT.md
- Feature specifications in phase plans
- Recent changes in SUMMARY.md files

## Feature Categorization

Group discovered features into test categories:

| Category | Examples | Priority |
|----------|----------|----------|
| Authentication | Login, logout, signup, password reset | Critical |
| Core Features | Main app functionality, primary user flows | Critical |
| Data Management | CRUD operations, forms, uploads | High |
| Navigation | Routing, menus, breadcrumbs | High |
| Secondary Features | Settings, preferences, profiles | Medium |
| Edge Cases | Error states, empty states, loading | Medium |
| Visual Polish | Animations, responsive design | Low |

## Test Workflow Generation

For each feature category, generate a test workflow file:

1. Name it `[category]-testing.md`
2. Include all related test cases
3. Order tests by dependency (setup first, teardown last)
4. Note authentication requirements upfront

## Example Analysis Output

After analyzing a project, produce:

```markdown
# Testing Analysis: [Project Name]

## Discovered Features

### Pages (8 found)
- `/` - Landing page (public)
- `/login` - Authentication (public)
- `/dashboard` - Main app (auth required)
- `/settings` - User settings (auth required)
- `/profile` - User profile (auth required)
- `/api/auth/*` - Auth API endpoints
- `/api/posts/*` - Posts CRUD API
- `/api/upload` - File upload API

### Interactive Components (12 found)
- LoginForm
- SignupForm
- PostEditor
- CommentSection
- UserAvatar
- NavigationMenu
- SettingsPanel
- FileUploader
- SearchBar
- NotificationBell
- ThemeToggle
- LogoutButton

### User Flows (from .planning/)
1. New user signup → email verification → first login
2. Create post → edit post → publish post
3. User settings → change password → logout

## Recommended Test Workflows

1. `auth-testing.md` - Login, logout, signup, password flows
2. `dashboard-testing.md` - Main dashboard features
3. `post-management-testing.md` - Post CRUD operations
4. `settings-testing.md` - User settings and preferences
5. `edge-cases-testing.md` - Error handling, empty states
```
