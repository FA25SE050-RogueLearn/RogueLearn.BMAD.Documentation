# RogueLearn Component Inventory

## Overview

This document provides a comprehensive inventory of all UI components used in the RogueLearn application. Each component is categorized, documented with its variants, states, and usage guidelines to ensure consistent implementation across the application.

## Core Components

### Button

**Description:** Interactive elements that trigger actions when clicked.

**Variants:**
- Primary: High-emphasis actions (color: `#4F46E5`)
- Secondary: Medium-emphasis actions (color: `#6B7280`)
- Tertiary/Ghost: Low-emphasis actions (transparent background)
- Danger: Destructive actions (color: `#DC2626`)
- Success: Confirmation actions (color: `#059669`)

**States:**
- Default
- Hover
- Active/Pressed
- Focused
- Disabled
- Loading

**Sizes:**
- Small: 32px height
- Medium (default): 40px height
- Large: 48px height

**Usage Guidelines:**
- Use primary buttons for main actions on a page
- Limit primary buttons to one per section
- Use secondary buttons for alternative actions
- Maintain consistent button ordering (primary on right for forms)
- Include loading states for asynchronous actions

### Card

**Description:** Container components that group related content and actions.

**Variants:**
- Standard: Basic content grouping
- Interactive: Entire card is clickable
- Elevated: With drop shadow for emphasis
- Bordered: With distinct border instead of shadow
- Quest Card: Specialized for quest display
- Achievement Card: Specialized for achievements

**States:**
- Default
- Hover (for interactive cards)
- Selected/Active
- Disabled

**Usage Guidelines:**
- Maintain consistent padding (16px standard)
- Use consistent heading hierarchy within cards
- Include clear call-to-action for interactive cards
- Limit content to maintain readability

### Input Field

**Description:** Form controls that allow users to enter text data.

**Variants:**
- Text Input
- Text Area
- Password Input
- Number Input
- Search Input
- Auto-complete Input

**States:**
- Default
- Focused
- Filled
- Error
- Disabled
- Read-only

**Usage Guidelines:**
- Always include visible labels
- Provide clear validation messages
- Use placeholder text sparingly
- Maintain consistent field widths within a form
- Include helper text for complex inputs

### Navigation Item

**Description:** Interactive elements used in navigation menus.

**Variants:**
- Primary Nav Item
- Secondary Nav Item
- Tab
- Breadcrumb Item
- Dropdown Item

**States:**
- Default
- Hover
- Active/Selected
- Disabled

**Usage Guidelines:**
- Use consistent iconography
- Provide clear visual feedback for current location
- Ensure adequate touch targets (min 44px)
- Use consistent spacing between items

### Tooltip

**Description:** Informational popups that appear on hover or focus.

**Variants:**
- Information
- Help
- Warning
- Error

**Positions:**
- Top
- Right
- Bottom
- Left

**Usage Guidelines:**
- Keep content brief and helpful
- Don't hide essential information in tooltips
- Ensure tooltips are keyboard accessible
- Use consistent delay timing (300ms standard)

### Badge

**Description:** Small indicators used to show status, count, or category.

**Variants:**
- Numeric: Shows count (notifications, items)
- Status: Shows state (new, completed, in progress)
- Tag: Shows category or attribute

**States:**
- Default
- Interactive (clickable)

**Usage Guidelines:**
- Keep text brief (1-2 words maximum)
- Use consistent colors for similar meanings
- Ensure adequate contrast with background
- Position consistently relative to parent elements

## Form Components

### Checkbox

**Description:** Selection control that allows multiple selections.

**Variants:**
- Standard
- Intermediate

**States:**
- Unchecked
- Checked
- Indeterminate
- Disabled
- Error

**Usage Guidelines:**
- Use for multi-select options
- Align checkboxes vertically for better scanning
- Use positive language in labels
- Allow clicking on label to toggle state

### Radio Button

**Description:** Selection control that allows a single selection from a group.

**Variants:**
- Standard
- Card Radio (larger, more visual)

**States:**
- Unselected
- Selected
- Disabled
- Error

**Usage Guidelines:**
- Use for mutually exclusive options
- Provide a default selection when possible
- Align radio buttons vertically for better scanning
- Use 3-7 options maximum for best usability

### Select / Dropdown

**Description:** Selection control for choosing from a list of options.

**Variants:**
- Single Select
- Multi-Select
- Searchable Select
- Combobox

**States:**
- Closed
- Open
- Disabled
- Error

**Usage Guidelines:**
- Use when there are more than 7 options
- Provide clear default or placeholder text
- Sort options in a logical order (alphabetical, numerical, etc.)
- Consider typeahead for long lists

### Toggle / Switch

**Description:** Binary control for turning options on or off.

**States:**
- Off
- On
- Disabled
- Loading

**Usage Guidelines:**
- Use for binary settings with immediate effect
- Position the toggle to the right of its label
- Use clear on/off labeling when necessary
- Provide immediate visual feedback on state change

### Slider

**Description:** Control for selecting a value from a range.

**Variants:**
- Single Value
- Range (min/max)
- Stepped
- Continuous

**States:**
- Default
- Focused/Dragging
- Disabled

**Usage Guidelines:**
- Use when exact values are less important than relative position
- Provide visible min/max labels
- Consider showing selected value
- Use appropriate step increments for context

## Navigation Components

### Sidebar Navigation

**Description:** Vertical navigation menu typically on the left side of the application.

**Variants:**
- Expanded
- Collapsed/Iconic
- Nested

**States:**
- Default
- With active item
- Expanded section
- Collapsed

**Usage Guidelines:**
- Organize items by frequency of use
- Limit to 7-9 top-level items
- Use consistent iconography
- Provide clear visual indication of current location

### Tabs

**Description:** Horizontal navigation for switching between related content sections.

**Variants:**
- Primary Tabs
- Secondary Tabs
- Underlined
- Contained
- Pills

**States:**
- Inactive
- Active
- Hover
- Disabled

**Usage Guidelines:**
- Use for content that belongs to the same context
- Limit to 2-7 tabs for readability
- Use clear, concise labels
- Maintain consistent tab order

### Breadcrumbs

**Description:** Horizontal navigation showing the user's location in the application hierarchy.

**Variants:**
- Text Only
- With Icons
- Collapsed (for mobile)

**States:**
- Default
- Hover
- Current (last item)

**Usage Guidelines:**
- Show full path from home to current page
- Use clear separators between items
- Make all items except current clickable
- Consider collapsing on mobile

### Pagination

**Description:** Navigation control for moving between pages of content.

**Variants:**
- Numbered
- Previous/Next Only
- Load More
- Infinite Scroll

**States:**
- Default
- Hover
- Active/Current
- Disabled

**Usage Guidelines:**
- Use consistent positioning (typically bottom of content)
- Show current page and total pages
- Consider context when choosing pagination style
- Ensure adequate touch targets for mobile

## Feedback Components

### Toast / Notification

**Description:** Temporary messages that appear to provide feedback about an operation.

**Variants:**
- Success
- Error
- Warning
- Information

**States:**
- Appearing
- Visible
- Disappearing

**Usage Guidelines:**
- Keep messages brief and actionable
- Position consistently (typically top-right)
- Auto-dismiss informational toasts after 5 seconds
- Require manual dismissal for errors

### Modal / Dialog

**Description:** Overlay windows that require user attention or input.

**Variants:**
- Confirmation
- Form
- Information
- Warning/Destructive
- Full-screen (mobile)

**States:**
- Opening
- Open
- Closing

**Usage Guidelines:**
- Use sparingly for important interruptions
- Provide clear titles and actions
- Allow dismissal via close button, overlay click, and ESC key
- Trap focus within modal for keyboard accessibility

### Progress Indicator

**Description:** Visual elements that show the progress of an operation.

**Variants:**
- Linear Progress Bar
- Circular Spinner
- Determinate (showing percentage)
- Indeterminate (animation only)
- Stepped Progress

**States:**
- Loading
- Complete
- Error

**Usage Guidelines:**
- Use determinate indicators when progress can be calculated
- Use indeterminate indicators for unknown duration
- Provide context about what's happening
- Consider skeleton screens for content loading

### Empty State

**Description:** Placeholder content shown when there is no data to display.

**Variants:**
- First-time Use
- No Results
- Error
- Coming Soon

**Usage Guidelines:**
- Include helpful illustration or icon
- Explain why the state exists
- Provide clear next steps or actions
- Maintain consistent styling with the application

## Data Display Components

### Table

**Description:** Structured display of data in rows and columns.

**Variants:**
- Standard
- Sortable
- Filterable
- Selectable Rows
- Expandable Rows

**States:**
- Default
- Hover Row
- Selected Row
- Sorted Column
- Loading

**Usage Guidelines:**
- Use for structured, comparable data
- Provide clear column headers
- Allow sorting for relevant columns
- Consider horizontal scrolling for many columns
- Include empty states

### List

**Description:** Vertical arrangement of items, often interactive.

**Variants:**
- Simple List
- Interactive List
- Icon List
- Avatar List
- Nested List

**States:**
- Default
- Hover
- Selected
- Disabled

**Usage Guidelines:**
- Use consistent item height
- Provide clear visual hierarchy within items
- Include appropriate spacing between items
- Consider virtualization for long lists

### Chart / Visualization

**Description:** Graphical representation of data.

**Variants:**
- Bar Chart
- Line Chart
- Pie/Donut Chart
- Area Chart
- Scatter Plot
- Skill Tree Visualization

**States:**
- Default
- Hover/Focus on data point
- Selected data point
- Loading
- Error

**Usage Guidelines:**
- Choose chart type appropriate for data relationship
- Include clear labels and legends
- Use consistent color coding
- Provide alternative text representation for accessibility
- Consider interactive elements for exploration

### Avatar

**Description:** Visual representation of a user or entity.

**Variants:**
- User Photo
- Initials
- Icon/Default
- Group

**Sizes:**
- Extra Small: 24px
- Small: 32px
- Medium: 40px
- Large: 48px
- Extra Large: 64px

**States:**
- Default
- With Status Indicator
- Interactive

**Usage Guidelines:**
- Use consistent sizing within contexts
- Provide fallback for missing images
- Consider status indicators when relevant
- Ensure adequate contrast for initials

## Specialized Components

### Skill Tree Node

**Description:** Interactive node representing a skill in the skill tree visualization.

**Variants:**
- Locked
- Available
- In Progress
- Completed
- Mastered

**States:**
- Default
- Hover
- Selected
- Animated (leveling up)

**Usage Guidelines:**
- Use clear visual differentiation between states
- Provide tooltips for additional information
- Ensure nodes are large enough for touch interaction
- Use consistent iconography for skill types

### Quest Card

**Description:** Specialized card for displaying quest information.

**Variants:**
- Main Quest
- Side Quest
- Daily Quest
- Boss Fight
- Guild Quest

**States:**
- Available
- In Progress
- Completed
- Failed
- Expired

**Usage Guidelines:**
- Include clear difficulty indicator
- Show estimated completion time
- Provide reward information
- Use consistent iconography for quest types
- Include progress indicator for active quests

### Notes Editor

**Description:** Rich text editor for creating and editing notes.

**Variants:**
- Simple (basic formatting)
- Advanced (with media embedding)
- Code-focused (with syntax highlighting)

**States:**
- Viewing
- Editing
- Saving
- Error

**Usage Guidelines:**
- Provide clear formatting controls
- Include auto-save functionality
- Show word/character count when relevant
- Ensure keyboard shortcuts for common actions
- Include responsive behavior for different screen sizes

### Achievement Badge

**Description:** Visual representation of user achievements.

**Variants:**
- Bronze
- Silver
- Gold
- Platinum
- Special/Limited Edition

**States:**
- Locked/Silhouette
- Unlocked
- Highlighted/Featured

**Usage Guidelines:**
- Use consistent visual language for achievement tiers
- Provide clear unlock criteria
- Include engaging animations for newly unlocked achievements
- Consider collection displays for multiple achievements

## Implementation Status

| Component | Design Status | Development Status | Storybook Documentation |
|-----------|---------------|-------------------|-------------------------|
| Button | Complete | Implemented | Complete |
| Card | Complete | Implemented | Complete |
| Input Field | Complete | Implemented | Complete |
| Navigation Item | Complete | Implemented | Complete |
| Tooltip | Complete | Implemented | Complete |
| Badge | Complete | Implemented | Complete |
| Checkbox | Complete | Implemented | Complete |
| Radio Button | Complete | Implemented | Complete |
| Select / Dropdown | Complete | Implemented | Complete |
| Toggle / Switch | Complete | Implemented | Complete |
| Slider | Complete | Implemented | Partial |
| Sidebar Navigation | Complete | Implemented | Complete |
| Tabs | Complete | Implemented | Complete |
| Breadcrumbs | Complete | Implemented | Complete |
| Pagination | Complete | Implemented | Partial |
| Toast / Notification | Complete | Implemented | Complete |
| Modal / Dialog | Complete | Implemented | Complete |
| Progress Indicator | Complete | Implemented | Complete |
| Empty State | Complete | Implemented | Partial |
| Table | Complete | Implemented | Partial |
| List | Complete | Implemented | Complete |
| Chart / Visualization | In Progress | Partial | Partial |
| Avatar | Complete | Implemented | Complete |
| Skill Tree Node | Complete | Implemented | Partial |
| Quest Card | Complete | Implemented | Complete |
| Notes Editor | Complete | Implemented | Partial |
| Achievement Badge | Complete | Implemented | Partial |

## Next Steps

1. Complete Storybook documentation for all components
2. Finalize chart/visualization components
3. Develop additional specialized components for AI features
4. Create component usage examples for developer reference
5. Implement automated accessibility testing for all components

## Appendix: Component Dependencies

| Component | Dependencies |
|-----------|-------------|
| Modal | Focus Trap, Portal, Overlay |
| Select | Popover, List, Input |
| Table | Pagination, Checkbox, Empty State |
| Skill Tree | Chart, Tooltip, Modal |
| Quest Card | Badge, Progress Indicator, Button |
| Notes Editor | Toolbar, Button, Modal |