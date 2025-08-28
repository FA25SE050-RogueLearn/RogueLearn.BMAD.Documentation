# Error Handling Visual Design & Messaging

## Overview

This document defines the visual design, messaging guidelines, and interaction patterns for error states in the RogueLearn application. Consistent error handling improves user experience by providing clear guidance when things go wrong while maintaining the RPG theme.

## Visual Design of Error States

### Error Components

#### 1. Inline Field Errors

- **Visual Treatment:**
  - Red outline around the input field (color: `#E53935`)
  - Error icon (exclamation mark in shield shape)
  - Error text appears below the field
  - Subtle shake animation on submission attempt (0.3s duration)

- **Anatomy:**
  ```
  +------------------------+
  |                        |
  | Input with red outline |
  |                        |
  +------------------------+
  🛡️ Error message here
  ```

- **States:**
  - **Initial:** No error styling
  - **Error:** Red outline, icon, and message
  - **Resolving:** Yellow outline during correction
  - **Resolved:** Green success outline (1s) then return to normal

#### 2. Form-Level Errors

- **Visual Treatment:**
  - Red banner at top of form (color: `#D32F2F`)
  - White text with shield icon
  - Subtle entrance animation (slide down and fade in)
  - Dismissible with X button

- **Anatomy:**
  ```
  +----------------------------------+
  | 🛡️ Form error message here     X |
  +----------------------------------+
  |                                  |
  |  Form fields                     |
  |                                  |
  +----------------------------------+
  ```

#### 3. Toast Notifications for System Errors

- **Visual Treatment:**
  - Slide-in notification from top-right
  - Red background for errors (color: `#C62828`)
  - White text with icon
  - Auto-dismiss after 5 seconds (configurable)
  - Progress bar indicating time remaining

- **Anatomy:**
  ```
  +----------------------------------+
  | 🛡️ System error message       X |
  | Secondary line with details      |
  | [Retry] [Details]                |
  +----------------------------------+
  ```

#### 4. Full-Page Error States

- **Visual Treatment:**
  - RPG-themed illustration (broken shield, defeated character)
  - Large, clear error heading
  - Supportive message with next steps
  - Action button(s) for recovery

- **Anatomy:**
  ```
  +----------------------------------+
  |                                  |
  |          [Illustration]          |
  |                                  |
  |          Error Heading           |
  |                                  |
  |      Supportive error message    |
  |      with recovery instructions  |
  |                                  |
  |         [Primary Action]         |
  |                                  |
  |      [Secondary Action Link]     |
  |                                  |
  +----------------------------------+
  ```

### Error State Color Palette

| Element | Regular State | Error State | Warning State | Success State |
|---------|---------------|-------------|---------------|---------------|
| Input Border | `#D1D5DB` | `#E53935` | `#F9A825` | `#43A047` |
| Background | `#FFFFFF` | `#FFEBEE` | `#FFF8E1` | `#E8F5E9` |
| Text | `#1F2937` | `#C62828` | `#F57F17` | `#2E7D32` |
| Icon | `#6B7280` | `#D32F2F` | `#FBC02D` | `#388E3C` |

### Typography for Error Messages

- **Error Headings:** 18px/24px, Semi-bold, Error Red
- **Error Messages:** 14px/20px, Regular, Dark Gray
- **Recovery Instructions:** 14px/20px, Regular, Dark Gray
- **Action Text:** 14px/20px, Medium, Primary Blue

### Iconography

- **Field Errors:** Shield with exclamation mark
- **System Errors:** Broken shield
- **Network Errors:** Shield with disconnected chain
- **Validation Errors:** Scroll with X mark
- **Success Recovery:** Shield with checkmark

## Error Message Guidelines

### Message Structure

1. **What happened** - Clear statement of the error
2. **Why it happened** - Brief explanation (when helpful)
3. **How to fix it** - Specific action user can take

### RPG-Themed Messaging Framework

| Error Type | Message Framework | Example |
|------------|-------------------|----------|
| Validation | "Your [item] lacks [requirement]. Add [specific detail] to continue your quest." | "Your password lacks magical strength. Add at least one special character to fortify it." |
| Network | "The connection to the Guild Hall has been lost. [Reason if known]. Try [action] to restore your connection." | "The connection to the Guild Hall has been lost. Your internet seems unstable. Try moving closer to your realm's connection point." |
| Server | "The Guild's archives are currently inaccessible. Our mages are working to restore access. Try again in a few moments." | |
| Permission | "You lack the required rank to access this area. Complete [prerequisite] to gain access." | "You lack the required rank to access this area. Complete the 'Database Fundamentals' quest line to gain access." |
| Input | "This [field] requires [format]. Adjust your input to proceed." | "This date requires MM/DD/YYYY format. Adjust your input to proceed." |

### Voice & Tone

- **Friendly but clear** - Never blame the user
- **Solution-oriented** - Always provide a next step
- **Concise** - Keep messages brief and scannable
- **RPG-themed** - Use consistent metaphors without being cheesy
- **Encouraging** - Frame as a temporary setback, not failure

### Localization Considerations

- Design for text expansion (messages may be 30% longer in some languages)
- Avoid idioms or cultural references that won't translate well
- Use sentence case for easier translation
- Maintain separate message files for each supported language

## Error State Interactions

### Form Validation

#### Real-time Validation

- Validate fields on blur (when user leaves field)
- Show success states for correctly completed fields
- Provide specific guidance for format requirements
- Use inline validation for immediate feedback

#### Submission Validation

- Prevent submission with blocking errors
- Scroll to and focus the first error field
- Group similar errors when possible
- Provide a summary for forms with many errors

### Recovery Patterns

#### Automatic Retries

- Automatically retry network requests up to 3 times
- Show loading state during retries
- Provide manual retry option after automatic attempts fail

#### Data Recovery

- Auto-save form data to prevent loss
- Offer to restore previous input after errors
- Provide clear recovery paths for all error states

#### Graceful Degradation

- Maintain core functionality when features fail
- Disable non-critical features rather than showing errors
- Provide alternative paths to complete tasks when possible

## Common Error States

### 1. Form Validation Errors

**Visual Example:**
```
+------------------------+
|                        |
| johndoe                |
|                        |
+------------------------+

+------------------------+
|                        |
| ******                 |
|                        |
+------------------------+
🛡️ Your password lacks magical strength. Add at least one special character.

+------------------------+
|                        |
| john@example           |
|                        |
+------------------------+
🛡️ Your spell is incomplete. Add ".com" to complete the email incantation.

      [  Create Account  ]
```

### 2. Network Connection Error

**Visual Example:**
```
+----------------------------------+
| 🔗 Connection to the Guild Hall  |
| has been lost. Attempting to     |
| reconnect...   [Try Again] [X]   |
+----------------------------------+
```

### 3. 404 Page Not Found

**Visual Example:**
```
+----------------------------------+
|                                  |
|     [Lost Adventurer Image]      |
|                                  |
|      This realm doesn't exist    |
|                                  |
|  You've ventured beyond the map  |
|  boundaries. This page doesn't   |
|  exist in the RogueLearn realm.  |
|                                  |
|     [Return to Your Journey]     |
|                                  |
|  [Report this to the Game Master]|
|                                  |
+----------------------------------+
```

### 4. Server Error

**Visual Example:**
```
+----------------------------------+
|                                  |
|     [Defeated Wizard Image]      |
|                                  |
|     The Guild Hall is down       |
|                                  |
|  Our master wizards are working  |
|  to restore the magical servers. |
|  Please try again soon.          |
|                                  |
|        [Try Again]               |
|                                  |
|  [Return to Dashboard]           |
|                                  |
+----------------------------------+
```

## Implementation Guidelines

### Component Library Integration

- Create reusable error components in the design system
- Document all error states in Storybook
- Provide code examples for common implementation patterns
- Create mixins/utilities for consistent error styling

### Accessibility Considerations

- Ensure 4.5:1 contrast ratio for all error text
- Use ARIA attributes to announce errors to screen readers
- Provide focus management to highlight errors
- Don't rely solely on color to indicate errors
- Ensure error messages are associated with their fields

### Testing Recommendations

- Test all error states with screen readers
- Verify keyboard navigation works for error resolution
- Test with users who have color vision deficiencies
- Validate error recovery flows with user testing

## Error Monitoring & Improvement

### Tracking Error Frequency

- Implement error tracking in analytics
- Monitor most common error types
- Track error resolution rates
- Identify patterns in error occurrence

### Continuous Improvement

- Review error messages quarterly
- Update based on user feedback and support tickets
- A/B test alternative error messages for problematic areas
- Maintain an error message style guide

## Conclusion

Consistent, helpful error handling is crucial to maintaining user trust and engagement. By following these guidelines, RogueLearn will provide a supportive experience even when things go wrong, keeping users engaged in their learning journey despite occasional setbacks.