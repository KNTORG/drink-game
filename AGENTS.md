# AGENTS.md - Development Guidelines for This Project

## Project Overview

This is a single-page HTML/CSS/JS dice game application (drinking game). It uses no build tools, frameworks, or package managers - just vanilla HTML, CSS, and JavaScript in a single file.

## Build/Lint/Test Commands

### Running the Project

Since this is a static HTML file, you can open it directly in a browser:

```bash
# Option 1: Open directly in browser
open dice-game.html
# or on Linux:
xdg-open dice-game.html

# Option 2: Serve locally (optional, for testing)
npx serve .
# or
python -m http.server 8000
```

### Linting

No linting tools are configured. For consistency, you may use:

```bash
# HTML validation
npx html-validate dice-game.html

# CSS linting
npx stylelint "**/*.html" --custom-syntax postcss-html

# JavaScript linting
npx eslint dice-game.html
```

### Testing

No automated tests exist. Manual testing should verify:

- Dice rolling animation completes correctly
- Result calculation is accurate for all dice combinations
- Settings panel toggles work
- History displays last 15 rolls
- Confetti and flash effects trigger properly

To add tests, consider adding a `test/` directory with:
- Unit tests for `getResult()` function
- Integration tests for UI interactions

```bash
# Example: Run Vitest (if added)
npm install -D vitest
npx vitest run
```

## Code Style Guidelines

### General Structure

- Single HTML file containing all HTML, CSS, and JavaScript
- CSS in `<style>` tag in `<head>`
- JavaScript in `<script>` tag at end of `<body>`
- Maintain existing line length (~80-120 chars where practical)

### HTML Conventions

- Use semantic HTML elements
- Use lowercase for tags and attributes
- Quote attribute values
- Maintain proper indentation (4 spaces)
- Include `lang` attribute on `<html>` tag
- Use self-closing tags for void elements

### CSS Conventions

- Use 4-space indentation
- Group related selectors together
- Use CSS custom properties for colors if needed
- Prefer flexbox and grid for layout
- Include vendor prefixes for mobile compatibility (`-webkit-`, `-moz-`)
- Use `rem` for font sizes, `px` for borders and small spacing

**Example:**
```css
.dice-container {
    width: 100px;
    height: 100px;
    perspective: 600px;
    position: relative;
}
```

### JavaScript Conventions

- Use `const` by default, `let` when needed, avoid `var`
- Use camelCase for variables and functions
- Use UPPER_SNAKE_CASE for constants
- Use meaningful variable names (Thai comments acceptable for user-facing strings)
- Avoid global variables where possible; wrap in IIFE or use module pattern
- Use ES6+ features (arrow functions, template literals, destructuring)

**Example:**
```javascript
const pipLayouts = {
    1: [0,0,0, 0,1,0, 0,0,0],
    // ...
};

function setPipLayout(diceFace, number) {
    // ...
}
```

### Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Variables | camelCase | `isRolling`, `fourPlusFourMode` |
| Constants | UPPER_SNAKE | `pipLayouts`, `faceRotations` |
| Functions | camelCase | `setPipLayout()`, `getResult()` |
| CSS Classes | kebab-case | `.dice-container`, `.roll-btn` |
| IDs | camelCase | `dice1`, `rollBtn` |

### Error Handling

- Use defensive checks for DOM elements that may not exist
- Wrap `querySelector`/`getElementById` calls with null checks when uncertain
- Use try-catch for operations that may fail (e.g., `JSON.parse`)
- Log errors to console for debugging

### Event Handling

- Use `addEventListener` for event binding
- Remove event listeners when cleaning up (if applicable)
- Use event delegation for repeated similar handlers
- Prevent default behavior with `e.preventDefault()` when needed

### Accessibility Considerations

- Include `aria-label` on icon-only buttons
- Use semantic buttons (`<button>`) not `<div>` with click handlers
- Ensure touch targets are at least 44x44px
- Consider `prefers-reduced-motion` for animations

### Performance Tips

- Cache DOM queries when used multiple times
- Use `transform` and `opacity` for animations (GPU accelerated)
- Remove event listeners and DOM elements when no longer needed
- Limit `querySelectorAll` calls in loops

## File Organization

Since this is a single-file project, all code lives in `dice-game.html`:

```
dice-game.html          # All HTML, CSS, JS (784 lines)
README.md              # Project documentation
```

If the project grows, consider splitting into:
```
index.html
css/styles.css
js/main.js
```

## Common Tasks

### Adding a New Dice Face
1. Add pip layout to `pipLayouts` object
2. Add rotation to `faceRotations` object
3. Add CSS transform for the face in HTML

### Modifying Game Rules
1. Edit `getResult()` function
2. Update the function's conditional logic
3. Test all dice combinations (1-6 × 1-6 = 36 cases)

### Adding Settings
1. Add HTML elements in `.settings-panel`
2. Add CSS for new elements
3. Add JavaScript event handlers
4. Update state variables as needed

## Notes for AI Agents

- This is a Thai-language application; all UI text is in Thai
- The app is designed for mobile-first touch interaction
- Uses 3D CSS transforms for dice animation
- No external dependencies (CDN-free)
- Data persists only in memory (no localStorage unless added)
