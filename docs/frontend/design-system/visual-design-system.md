# RogueLearn Visual Design System

## Overview

This document defines the comprehensive visual design system for the RogueLearn application. It establishes the visual language that creates a cohesive, accessible, and engaging user experience while maintaining the RPG theme throughout the interface.

## Color Palette

### Primary Colors

| Color Name | Hex Code | RGB | Usage |
|------------|----------|-----|-------|
| Magical Blue | `#4F46E5` | rgb(79, 70, 229) | Primary actions, key interactive elements |
| Stone Gray | `#6B7280` | rgb(107, 114, 128) | Secondary actions, supporting elements |
| Mystical Purple | `#8B5CF6` | rgb(139, 92, 246) | Accent elements, highlights |

### Semantic Colors

| Color Name | Hex Code | RGB | Usage |
|------------|----------|-----|-------|
| Forest Green | `#059669` | rgb(5, 150, 105) | Success states, completion |
| Dragon Red | `#DC2626` | rgb(220, 38, 38) | Error states, destructive actions |
| Amber Warning | `#D97706` | rgb(217, 119, 6) | Warning states, caution |
| Info Blue | `#3B82F6` | rgb(59, 130, 246) | Information, help |

### Background Colors

| Color Name | Hex Code | RGB | Usage |
|------------|----------|-----|-------|
| Dark Dungeon | `#1F2937` | rgb(31, 41, 55) | Main background |
| Lighter Dungeon | `#374151` | rgb(55, 65, 81) | Secondary background, cards |
| Darkest Dungeon | `#111827` | rgb(17, 24, 39) | Tertiary background, modals |

### Text Colors

| Color Name | Hex Code | RGB | Usage |
|------------|----------|-----|-------|
| Scroll Parchment | `#F9FAFB` | rgb(249, 250, 251) | Primary text |
| Faded Parchment | `#D1D5DB` | rgb(209, 213, 219) | Secondary text |
| Ancient Ink | `#9CA3AF` | rgb(156, 163, 175) | Disabled text |

### Accessibility Considerations

- All color combinations meet WCAG 2.1 AA standards with a minimum contrast ratio of 4.5:1 for normal text and 3:1 for large text
- Interactive elements maintain a 3:1 contrast ratio against adjacent colors
- Color is never used as the sole means of conveying information
- A high contrast mode is available for users who need additional contrast

## Typography

### Font Families

- **Primary Font:** Inter (Sans-serif)
  - Used for body text, UI elements, and general content
  - Provides excellent readability at various sizes

- **Heading Font:** Crimson Pro (Serif)
  - Used for headings and RPG-themed elements
  - Creates a fantasy/medieval aesthetic while maintaining readability

- **Monospace Font:** JetBrains Mono
  - Used for code snippets, technical information
  - Optimized for readability of code and technical content

### Type Scale

| Name | Size | Line Height | Weight | Usage |
|------|------|-------------|--------|-------|
| Heading 1 | 2.5rem (40px) | 1.2 (48px) | 700 | Main page headings |
| Heading 2 | 2rem (32px) | 1.2 (38px) | 700 | Section headings |
| Heading 3 | 1.5rem (24px) | 1.3 (31px) | 600 | Subsection headings |
| Heading 4 | 1.25rem (20px) | 1.4 (28px) | 600 | Card headings |
| Heading 5 | 1.125rem (18px) | 1.4 (25px) | 600 | Minor headings |
| Body Large | 1.125rem (18px) | 1.5 (27px) | 400 | Featured content |
| Body | 1rem (16px) | 1.5 (24px) | 400 | Main content text |
| Body Small | 0.875rem (14px) | 1.5 (21px) | 400 | Secondary content |
| Caption | 0.75rem (12px) | 1.5 (18px) | 400 | Supporting text, labels |

### Typography Hierarchy

- Clear hierarchy is maintained through size, weight, and spacing
- Headings use the serif font (Crimson Pro) to reinforce the RPG theme
- Body text uses the sans-serif font (Inter) for optimal readability
- Line length is limited to 70-80 characters for optimal readability

### Accessibility Considerations

- Text can be resized up to 200% without loss of content or functionality
- Line heights are set to improve readability for users with cognitive disabilities
- Font weights ensure sufficient contrast between different text elements
- All text maintains the required contrast ratios against backgrounds

## Iconography

### Icon System

RogueLearn uses a custom icon system that blends modern UI conventions with RPG-themed elements.

### Icon Styles

- **Line Icons:** Used for navigation, actions, and UI controls
  - 2px stroke weight
  - Rounded caps and joins
  - 24×24px default size

- **Solid Icons:** Used for status indicators and selected states
  - Filled shapes with consistent visual weight
  - 24×24px default size

- **RPG-Themed Icons:** Used for game elements and special features
  - Stylized to match fantasy/medieval aesthetic
  - Consistent with overall visual language

### Icon Sizes

| Size | Dimensions | Usage |
|------|------------|-------|
| X-Small | 16×16px | Dense UIs, tight spaces |
| Small | 20×20px | Secondary actions, supporting elements |
| Medium | 24×24px | Primary actions, navigation |
| Large | 32×32px | Featured elements, primary navigation |
| X-Large | 48×48px | Hero elements, illustrations |

### Icon Categories

- **Navigation:** Home, back, forward, menu
- **Actions:** Add, edit, delete, save
- **Communication:** Message, notification, share
- **Media:** Play, pause, download, upload
- **RPG Elements:** Skill, quest, achievement, character

### Accessibility Considerations

- Icons used for interactive elements always include text labels or tooltips
- Icons maintain a minimum touch target size of 44×44px
- Decorative icons are properly marked as such for screen readers
- Critical icons have text alternatives

## Spacing and Layout

### Spacing Scale

A consistent 4px base unit is used throughout the interface.

| Name | Size | Usage |
|------|------|-------|
| 2xs | 4px | Minimal spacing, tight UIs |
| xs | 8px | Compact elements, icons |
| sm | 12px | Form controls, buttons |
| md | 16px | Standard spacing, margins |
| lg | 24px | Section spacing |
| xl | 32px | Major section divisions |
| 2xl | 48px | Page sections |
| 3xl | 64px | Major layout divisions |

### Layout Grid

- 12-column grid system for desktop layouts
- 8-column grid for tablet layouts
- 4-column grid for mobile layouts
- 16px (md) gutters between columns
- Responsive breakpoints:
  - Mobile: 320px - 767px
  - Tablet: 768px - 1023px
  - Desktop: 1024px+

### Component Spacing

- Consistent internal padding within components
- Standard card padding: 16px (md)
- Form field spacing: 24px (lg) between fields
- Button padding: 8px 16px (xs md) for standard buttons

## Animation and Motion

### Motion Principles

- **Purposeful:** Animations serve a functional purpose
- **Responsive:** Animations respond to user actions
- **Subtle:** Animations enhance rather than distract
- **Consistent:** Similar elements animate in similar ways

### Timing and Easing

| Duration | Usage |
|----------|-------|
| Fast (150ms) | Micro-interactions, button states |
| Medium (300ms) | Standard transitions, reveals |
| Slow (500ms) | Major transitions, emphasis |

| Easing | Usage |
|--------|-------|
| Ease-out (cubic-bezier(0.0, 0.0, 0.2, 1)) | Elements entering the screen |
| Ease-in (cubic-bezier(0.4, 0.0, 1, 1)) | Elements exiting the screen |
| Ease-in-out (cubic-bezier(0.4, 0.0, 0.2, 1)) | Elements moving within the screen |

### Common Animations

- **Fade:** Opacity transitions for appearing/disappearing elements
- **Scale:** Size transitions for emphasis
- **Slide:** Position transitions for panels and drawers
- **Pulse:** Attention-grabbing highlights
- **Magical Effects:** Particle effects, glows for RPG-themed interactions

### Reduced Motion Considerations

- All animations respect the user's reduced motion preferences
- Essential animations are simplified when reduced motion is enabled
- No animations that could trigger vestibular disorders

## Implementation Guidelines

### CSS Variables

All design tokens are implemented as CSS custom properties for consistent application:

```css
:root {
  /* Colors */
  --color-primary: #4F46E5;
  --color-secondary: #6B7280;
  --color-accent: #8B5CF6;
  
  /* Typography */
  --font-family-base: 'Inter', sans-serif;
  --font-family-heading: 'Crimson Pro', serif;
  
  /* Spacing */
  --space-2xs: 4px;
  --space-xs: 8px;
  --space-sm: 12px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
}
```

### Component Implementation

- All components should use the design tokens defined in this document
- Avoid hard-coded values for colors, spacing, typography
- Use the component library documented in the [Component Inventory](../components/component-inventory.md)

### Tailwind Configuration

The design system is implemented in Tailwind CSS with custom theme extensions:

```js
// tailwind.config.js example
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: '#4F46E5',
        secondary: '#6B7280',
        accent: '#8B5CF6',
        // Additional colors...
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
        serif: ['Crimson Pro', 'serif'],
        mono: ['JetBrains Mono', 'monospace'],
      },
      // Additional theme extensions...
    }
  }
}
```

## Design Assets

### Icon Library

The complete icon library is available in the design system Figma file and as SVG assets in the codebase.

### Design Files

- Figma design system file with component library
- SVG icon set
- Color palette swatches

## Versioning and Updates

The design system follows semantic versioning:

- **Major version:** Breaking changes to the design system
- **Minor version:** New additions that don't break existing components
- **Patch version:** Bug fixes and minor updates

All changes to the design system are documented in the changelog.