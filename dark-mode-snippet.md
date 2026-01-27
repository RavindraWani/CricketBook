# Dark Mode Implementation - Global Setup

This dark mode toggle system works **globally** across all pages using localStorage. Once a user selects their preference, it persists across all pages.

## ✅ Already Implemented in:
- livescore.html

## 📋 To Add to Other Pages:

### Step 1: Add CSS Variables (in `<style>` section)

Add this **at the very beginning** of your `<style>` tag, before any other CSS:

```css
:root {
  /* Light mode (default) */
  --bg-gradient-start: #667eea;
  --bg-gradient-end: #764ba2;
  --card-bg: #ffffff;
  --card-border: rgba(148,163,184,.2);
  --text-primary: #1f2937;
  --text-secondary: #6b7280;
  --text-tertiary: #94a3b8;
  --nav-text: #ffffff;
  --badge-bg: rgba(255, 255, 255, 0.15);
  --badge-border: rgba(255, 255, 255, 0.1);
  --badge-hover: rgba(255, 255, 255, 0.25);
  --table-bg: #f8fafc;
  --table-border: #e2e8f0;
  --empty-bg: #ffffff;
  --error-bg: #fef2f2;
  --error-border: #fecaca;
  --error-text: #991b1b;
}

[data-theme="dark"] {
  /* Dark mode */
  --bg-gradient-start: #1e293b;
  --bg-gradient-end: #0f172a;
  --card-bg: #1e293b;
  --card-border: rgba(71,85,105,.3);
  --text-primary: #f1f5f9;
  --text-secondary: #cbd5e1;
  --text-tertiary: #64748b;
  --nav-text: #f1f5f9;
  --badge-bg: rgba(255, 255, 255, 0.1);
  --badge-border: rgba(255, 255, 255, 0.05);
  --badge-hover: rgba(255, 255, 255, 0.2);
  --table-bg: #0f172a;
  --table-border: #334155;
  --empty-bg: #1e293b;
  --error-bg: #450a0a;
  --error-border: #7f1d1d;
  --error-text: #fca5a5;
}
```

### Step 2: Update Your Existing Styles

Replace hardcoded colors with CSS variables:

**Before:**
```css
body {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: #1f2937;
}
```

**After:**
```css
body {
  background: linear-gradient(135deg, var(--bg-gradient-start) 0%, var(--bg-gradient-end) 100%);
  color: var(--text-primary);
  transition: background 0.3s ease, color 0.3s ease;
}
```

**Common replacements:**
- `background: #ffffff` → `background: var(--card-bg)`
- `color: #1f2937` → `color: var(--text-primary)`
- `color: #6b7280` → `color: var(--text-secondary)`
- `color: white` (for nav items) → `color: var(--nav-text)`
- `border: 1px solid #e2e8f0` → `border: 1px solid var(--table-border)`

### Step 3: Add Theme Toggle Button Styles

Add this CSS for the toggle button:

```css
/* Dark Mode Toggle */
.theme-toggle {
  background: var(--badge-bg);
  border: 1px solid var(--badge-border);
  color: var(--nav-text);
  font-size: 20px;
  padding: 8px 12px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.2s;
  width: 44px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-right: 8px;
}

.theme-toggle:hover {
  background: var(--badge-hover);
  transform: scale(1.05);
}

.theme-toggle:active {
  transform: scale(0.95);
}
```

### Step 4: Add JavaScript Functions

Add this script **before the closing `</head>` tag** or at the beginning of your existing `<script>` section:

```javascript
// Dark Mode Management (Global - works across all pages)
function initTheme() {
  const savedTheme = localStorage.getItem('cricketbook_theme') || 'light';
  document.documentElement.setAttribute('data-theme', savedTheme);
  updateThemeIcon(savedTheme);
}

function toggleTheme() {
  const currentTheme = document.documentElement.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
  
  document.documentElement.setAttribute('data-theme', newTheme);
  localStorage.setItem('cricketbook_theme', newTheme);
  updateThemeIcon(newTheme);
  
  console.log('🌓 Theme switched to:', newTheme);
}

function updateThemeIcon(theme) {
  const themeToggle = document.getElementById('themeToggle');
  if (themeToggle) {
    themeToggle.textContent = theme === 'dark' ? '☀️' : '🌙';
    themeToggle.title = theme === 'dark' ? 'Switch to Light Mode' : 'Switch to Dark Mode';
  }
}

// Initialize theme on page load
initTheme();
```

### Step 5: Add Toggle Button to Navigation

Update your navigation header HTML:

**Before:**
```html
<div class="nav-header">
  <a href="index.html" class="nav-title">🏏 CricketBook</a>
  <button class="nav-toggle" onclick="toggleNav()">
    <span class="hamburger">☰</span>
    <span class="close">×</span>
  </button>
</div>
```

**After:**
```html
<div class="nav-header">
  <a href="index.html" class="nav-title">🏏 CricketBook</a>
  <div style="display: flex; align-items: center;">
    <button class="theme-toggle" id="themeToggle" onclick="toggleTheme()" title="Toggle Dark Mode">
      🌙
    </button>
    <button class="nav-toggle" onclick="toggleNav()">
      <span class="hamburger">☰</span>
      <span class="close">×</span>
    </button>
  </div>
</div>
```

## 🎯 How It Works

1. **localStorage persistence**: The user's theme choice is saved in `localStorage` with key `cricketbook_theme`
2. **Cross-page sync**: When any page loads, it checks localStorage and applies the saved theme
3. **Instant switching**: Click the moon 🌙 icon to switch to dark mode, or sun ☀️ to switch to light mode
4. **CSS Variables**: All colors use CSS custom properties that change based on the `data-theme` attribute
5. **Smooth transitions**: CSS transitions make the theme switch smooth

## 🎨 Customization

You can adjust dark mode colors by modifying the `[data-theme="dark"]` section in the CSS variables. For example:

```css
[data-theme="dark"] {
  --bg-gradient-start: #1a1a2e;  /* Change to your preferred dark color */
  --bg-gradient-end: #16213e;     /* Change to your preferred dark color */
  /* ... other variables ... */
}
```

## 📱 Mobile Friendly

The toggle button is 44x44px (recommended touch target size) and works perfectly on mobile devices.

## ♿ Accessibility

- The button has a descriptive `title` attribute
- Clear visual feedback on hover and click
- High contrast maintained in both themes
