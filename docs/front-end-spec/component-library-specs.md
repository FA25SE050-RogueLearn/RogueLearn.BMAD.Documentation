# RogueLearn Component Library & Design System
## Comprehensive UI Component Specifications

---

## Table of Contents

1. [Design System Overview](#1-design-system-overview)
2. [Design Tokens](#2-design-tokens)
3. [Core Components](#3-core-components)
4. [Layout Components](#4-layout-components)
5. [Form Components](#5-form-components)
6. [Data Display Components](#6-data-display-components)
7. [Navigation Components](#7-navigation-components)
8. [Feedback Components](#8-feedback-components)
9. [Feature-Specific Components](#9-feature-specific-components)
10. [Component Usage Guidelines](#10-component-usage-guidelines)

---

## 1. Design System Overview

### 1.1 Design Philosophy

**Core Principles:**
- 🎯 **User-Centric**: Every component serves user needs first
- 🔄 **Consistency**: Uniform behavior and appearance across the platform
- ♿ **Accessibility**: WCAG 2.1 AA compliance by default
- 📱 **Responsive**: Mobile-first, adaptive design
- ⚡ **Performance**: Optimized for speed and efficiency
- 🎨 **Scalable**: Easy to extend and customize

### 1.2 Component Architecture

```typescript
// Base Component Interface
interface BaseComponentProps {
  className?: string;
  children?: React.ReactNode;
  testId?: string;
  'aria-label'?: string;
  'aria-describedby'?: string;
}

// Component Variants System
type ComponentVariant = 'primary' | 'secondary' | 'success' | 'warning' | 'error';
type ComponentSize = 'xs' | 'sm' | 'md' | 'lg' | 'xl';
type ComponentState = 'default' | 'hover' | 'active' | 'disabled' | 'loading';
```

### 1.3 Naming Convention

```typescript
// Component Naming Pattern
// [Feature][Component][Variant?]
// Examples:
// - Button, ButtonPrimary, ButtonSecondary
// - Card, CardElevated, CardOutlined
// - Input, InputSearch, InputPassword
// - Modal, ModalConfirm, ModalFullscreen
```

---

## 2. Design Tokens

### 2.1 Color System

```typescript
// Color Palette
export const colors = {
  // Primary Colors (RogueLearn Brand)
  primary: {
    50: '#f0f9ff',
    100: '#e0f2fe',
    200: '#bae6fd',
    300: '#7dd3fc',
    400: '#38bdf8',
    500: '#0ea5e9', // Primary brand color
    600: '#0284c7',
    700: '#0369a1',
    800: '#075985',
    900: '#0c4a6e',
  },
  
  // Secondary Colors (Accent)
  secondary: {
    50: '#fdf4ff',
    100: '#fae8ff',
    200: '#f5d0fe',
    300: '#f0abfc',
    400: '#e879f9',
    500: '#d946ef', // Secondary accent
    600: '#c026d3',
    700: '#a21caf',
    800: '#86198f',
    900: '#701a75',
  },
  
  // Semantic Colors
  success: {
    50: '#f0fdf4',
    100: '#dcfce7',
    200: '#bbf7d0',
    300: '#86efac',
    400: '#4ade80',
    500: '#22c55e', // Success green
    600: '#16a34a',
    700: '#15803d',
    800: '#166534',
    900: '#14532d',
  },
  
  warning: {
    50: '#fffbeb',
    100: '#fef3c7',
    200: '#fde68a',
    300: '#fcd34d',
    400: '#fbbf24',
    500: '#f59e0b', // Warning amber
    600: '#d97706',
    700: '#b45309',
    800: '#92400e',
    900: '#78350f',
  },
  
  error: {
    50: '#fef2f2',
    100: '#fee2e2',
    200: '#fecaca',
    300: '#fca5a5',
    400: '#f87171',
    500: '#ef4444', // Error red
    600: '#dc2626',
    700: '#b91c1c',
    800: '#991b1b',
    900: '#7f1d1d',
  },
  
  // Neutral Colors
  gray: {
    50: '#f9fafb',
    100: '#f3f4f6',
    200: '#e5e7eb',
    300: '#d1d5db',
    400: '#9ca3af',
    500: '#6b7280',
    600: '#4b5563',
    700: '#374151',
    800: '#1f2937',
    900: '#111827',
  },
};
```

### 2.2 Typography System

```typescript
// Typography Scale
export const typography = {
  fontFamily: {
    sans: ['Inter', 'system-ui', 'sans-serif'],
    mono: ['JetBrains Mono', 'Consolas', 'monospace'],
  },
  
  fontSize: {
    xs: ['0.75rem', { lineHeight: '1rem' }],      // 12px
    sm: ['0.875rem', { lineHeight: '1.25rem' }],  // 14px
    base: ['1rem', { lineHeight: '1.5rem' }],     // 16px
    lg: ['1.125rem', { lineHeight: '1.75rem' }],  // 18px
    xl: ['1.25rem', { lineHeight: '1.75rem' }],   // 20px
    '2xl': ['1.5rem', { lineHeight: '2rem' }],    // 24px
    '3xl': ['1.875rem', { lineHeight: '2.25rem' }], // 30px
    '4xl': ['2.25rem', { lineHeight: '2.5rem' }], // 36px
    '5xl': ['3rem', { lineHeight: '1' }],         // 48px
    '6xl': ['3.75rem', { lineHeight: '1' }],      // 60px
  },
  
  fontWeight: {
    thin: '100',
    extralight: '200',
    light: '300',
    normal: '400',
    medium: '500',
    semibold: '600',
    bold: '700',
    extrabold: '800',
    black: '900',
  },
};
```

### 2.3 Spacing System

```typescript
// Spacing Scale (based on 4px grid)
export const spacing = {
  0: '0px',
  1: '0.25rem',   // 4px
  2: '0.5rem',    // 8px
  3: '0.75rem',   // 12px
  4: '1rem',      // 16px
  5: '1.25rem',   // 20px
  6: '1.5rem',    // 24px
  8: '2rem',      // 32px
  10: '2.5rem',   // 40px
  12: '3rem',     // 48px
  16: '4rem',     // 64px
  20: '5rem',     // 80px
  24: '6rem',     // 96px
  32: '8rem',     // 128px
  40: '10rem',    // 160px
  48: '12rem',    // 192px
  56: '14rem',    // 224px
  64: '16rem',    // 256px
};
```

### 2.4 Border Radius & Shadows

```typescript
// Border Radius
export const borderRadius = {
  none: '0px',
  sm: '0.125rem',   // 2px
  base: '0.25rem',  // 4px
  md: '0.375rem',   // 6px
  lg: '0.5rem',     // 8px
  xl: '0.75rem',    // 12px
  '2xl': '1rem',    // 16px
  '3xl': '1.5rem',  // 24px
  full: '9999px',
};

// Box Shadows
export const boxShadow = {
  sm: '0 1px 2px 0 rgb(0 0 0 / 0.05)',
  base: '0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1)',
  md: '0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)',
  lg: '0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)',
  xl: '0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1)',
  '2xl': '0 25px 50px -12px rgb(0 0 0 / 0.25)',
  inner: 'inset 0 2px 4px 0 rgb(0 0 0 / 0.05)',
};
```

---

## 3. Core Components

### 3.1 Button Component

```typescript
// Button Component Specification
interface ButtonProps extends BaseComponentProps {
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost' | 'link';
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  disabled?: boolean;
  loading?: boolean;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
  fullWidth?: boolean;
  onClick?: (event: React.MouseEvent<HTMLButtonElement>) => void;
  type?: 'button' | 'submit' | 'reset';
}

// Usage Examples
<Button variant="primary" size="md">
  Create Party
</Button>

<Button variant="outline" leftIcon={<PlusIcon />}>
  Add Member
</Button>

<Button variant="ghost" loading>
  Saving...
</Button>
```

**Visual Specifications:**
- **Primary**: Blue background (#0ea5e9), white text, hover darkens
- **Secondary**: Purple background (#d946ef), white text, hover darkens
- **Outline**: Transparent background, colored border, hover fills
- **Ghost**: Transparent background, colored text, hover adds background
- **Link**: No background, colored text, underline on hover

**Accessibility:**
- Minimum 44px touch target on mobile
- Focus ring with 2px outline
- Screen reader compatible
- Keyboard navigation support

### 3.2 Input Component

```typescript
// Input Component Specification
interface InputProps extends BaseComponentProps {
  type?: 'text' | 'email' | 'password' | 'number' | 'tel' | 'url' | 'search';
  placeholder?: string;
  value?: string;
  defaultValue?: string;
  disabled?: boolean;
  readOnly?: boolean;
  required?: boolean;
  error?: boolean;
  helperText?: string;
  leftIcon?: React.ReactNode;
  rightIcon?: React.ReactNode;
  size?: 'sm' | 'md' | 'lg';
  fullWidth?: boolean;
  onChange?: (event: React.ChangeEvent<HTMLInputElement>) => void;
  onFocus?: (event: React.FocusEvent<HTMLInputElement>) => void;
  onBlur?: (event: React.FocusEvent<HTMLInputElement>) => void;
}

// Usage Examples
<Input
  type="email"
  placeholder="Enter your email"
  leftIcon={<EnvelopeIcon />}
  helperText="We'll never share your email"
/>

<Input
  type="password"
  error={!!errors.password}
  helperText={errors.password?.message}
  rightIcon={<EyeIcon />}
/>
```

### 3.3 Card Component

```typescript
// Card Component Specification
interface CardProps extends BaseComponentProps {
  variant?: 'default' | 'outlined' | 'elevated';
  padding?: 'none' | 'sm' | 'md' | 'lg';
  hoverable?: boolean;
  clickable?: boolean;
  onClick?: () => void;
}

// Card Header Component
interface CardHeaderProps extends BaseComponentProps {
  title: string;
  subtitle?: string;
  actions?: React.ReactNode;
  avatar?: React.ReactNode;
}

// Usage Examples
<Card variant="elevated" hoverable>
  <CardHeader
    title="Study Party"
    subtitle="5 members active"
    actions={<Button variant="ghost">Join</Button>}
  />
  <CardContent>
    <p>Join us for collaborative learning!</p>
  </CardContent>
</Card>
```

---

## 4. Layout Components

### 4.1 Container Component

```typescript
// Container Component Specification
interface ContainerProps extends BaseComponentProps {
  maxWidth?: 'sm' | 'md' | 'lg' | 'xl' | '2xl' | 'full';
  padding?: boolean;
  center?: boolean;
}

// Usage Examples
<Container maxWidth="lg" padding>
  <h1>Page Content</h1>
</Container>
```

### 4.2 Grid System

```typescript
// Grid Component Specification
interface GridProps extends BaseComponentProps {
  cols?: 1 | 2 | 3 | 4 | 5 | 6 | 12;
  gap?: 'none' | 'sm' | 'md' | 'lg' | 'xl';
  responsive?: {
    sm?: number;
    md?: number;
    lg?: number;
    xl?: number;
  };
}

interface GridItemProps extends BaseComponentProps {
  span?: number;
  offset?: number;
  order?: number;
}

// Usage Examples
<Grid cols={3} gap="md">
  <GridItem span={2}>
    <Card>Main Content</Card>
  </GridItem>
  <GridItem span={1}>
    <Card>Sidebar</Card>
  </GridItem>
</Grid>
```

### 4.3 Stack Component

```typescript
// Stack Component Specification
interface StackProps extends BaseComponentProps {
  direction?: 'horizontal' | 'vertical';
  spacing?: 'none' | 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  align?: 'start' | 'center' | 'end' | 'stretch';
  justify?: 'start' | 'center' | 'end' | 'between' | 'around' | 'evenly';
  wrap?: boolean;
}

// Usage Examples
<Stack direction="horizontal" spacing="md" align="center">
  <Button variant="primary">Save</Button>
  <Button variant="outline">Cancel</Button>
</Stack>
```

---

## 5. Form Components

### 5.1 Form Field Component

```typescript
// Form Field Component Specification
interface FormFieldProps extends BaseComponentProps {
  label: string;
  required?: boolean;
  error?: boolean;
  helperText?: string;
  children: React.ReactNode;
}

// Usage Examples
<FormField
  label="Party Name"
  required
  error={!!errors.name}
  helperText={errors.name?.message}
>
  <Input
    name="name"
    placeholder="Enter party name"
    {...register('name')}
  />
</FormField>
```

### 5.2 Select Component

```typescript
// Select Component Specification
interface SelectOption {
  value: string | number;
  label: string;
  disabled?: boolean;
  icon?: React.ReactNode;
}

interface SelectProps extends BaseComponentProps {
  options: SelectOption[];
  value?: string | number;
  defaultValue?: string | number;
  placeholder?: string;
  disabled?: boolean;
  error?: boolean;
  multiple?: boolean;
  searchable?: boolean;
  clearable?: boolean;
  size?: 'sm' | 'md' | 'lg';
  onChange?: (value: string | number | (string | number)[]) => void;
}

// Usage Examples
<Select
  options={[
    { value: 'beginner', label: 'Beginner' },
    { value: 'intermediate', label: 'Intermediate' },
    { value: 'advanced', label: 'Advanced' },
  ]}
  placeholder="Select difficulty level"
  searchable
/>
```

### 5.3 Checkbox & Radio Components

```typescript
// Checkbox Component Specification
interface CheckboxProps extends BaseComponentProps {
  checked?: boolean;
  defaultChecked?: boolean;
  disabled?: boolean;
  indeterminate?: boolean;
  label?: string;
  description?: string;
  size?: 'sm' | 'md' | 'lg';
  onChange?: (checked: boolean) => void;
}

// Radio Component Specification
interface RadioProps extends BaseComponentProps {
  name: string;
  value: string;
  checked?: boolean;
  disabled?: boolean;
  label?: string;
  description?: string;
  size?: 'sm' | 'md' | 'lg';
  onChange?: (value: string) => void;
}

// Usage Examples
<Checkbox
  label="Make party private"
  description="Only invited members can join"
  onChange={(checked) => setIsPrivate(checked)}
/>

<RadioGroup name="difficulty" onChange={setDifficulty}>
  <Radio value="easy" label="Easy" />
  <Radio value="medium" label="Medium" />
  <Radio value="hard" label="Hard" />
</RadioGroup>
```

---

## 6. Data Display Components

### 6.1 Table Component

```typescript
// Table Component Specification
interface TableColumn<T> {
  key: keyof T;
  title: string;
  width?: string | number;
  align?: 'left' | 'center' | 'right';
  sortable?: boolean;
  render?: (value: any, record: T, index: number) => React.ReactNode;
}

interface TableProps<T> extends BaseComponentProps {
  data: T[];
  columns: TableColumn<T>[];
  loading?: boolean;
  pagination?: {
    current: number;
    pageSize: number;
    total: number;
    onChange: (page: number, pageSize: number) => void;
  };
  selection?: {
    selectedRowKeys: string[];
    onChange: (selectedRowKeys: string[]) => void;
  };
  onRowClick?: (record: T, index: number) => void;
}

// Usage Examples
<Table
  data={members}
  columns={[
    {
      key: 'name',
      title: 'Name',
      render: (name, record) => (
        <div className="flex items-center gap-2">
          <Avatar src={record.avatar} />
          <span>{name}</span>
        </div>
      ),
    },
    {
      key: 'role',
      title: 'Role',
      render: (role) => <Badge variant={role}>{role}</Badge>,
    },
    {
      key: 'actions',
      title: 'Actions',
      render: (_, record) => (
        <Button variant="ghost" size="sm">
          Edit
        </Button>
      ),
    },
  ]}
  pagination={{
    current: 1,
    pageSize: 10,
    total: 100,
    onChange: handlePageChange,
  }}
/>
```

### 6.2 List Component

```typescript
// List Component Specification
interface ListItemProps extends BaseComponentProps {
  title: string;
  description?: string;
  avatar?: React.ReactNode;
  actions?: React.ReactNode;
  onClick?: () => void;
}

interface ListProps extends BaseComponentProps {
  items: ListItemProps[];
  loading?: boolean;
  emptyText?: string;
  divider?: boolean;
}

// Usage Examples
<List
  items={[
    {
      title: 'JavaScript Fundamentals',
      description: 'Learn the basics of JavaScript programming',
      avatar: <Avatar>JS</Avatar>,
      actions: <Button variant="outline">Join</Button>,
    },
    {
      title: 'React Advanced Patterns',
      description: 'Master advanced React concepts',
      avatar: <Avatar>React</Avatar>,
      actions: <Button variant="outline">Join</Button>,
    },
  ]}
  divider
/>
```

### 6.3 Badge & Tag Components

```typescript
// Badge Component Specification
interface BadgeProps extends BaseComponentProps {
  variant?: 'primary' | 'secondary' | 'success' | 'warning' | 'error' | 'neutral';
  size?: 'sm' | 'md' | 'lg';
  dot?: boolean;
  count?: number;
  showZero?: boolean;
}

// Tag Component Specification
interface TagProps extends BaseComponentProps {
  variant?: 'primary' | 'secondary' | 'success' | 'warning' | 'error' | 'neutral';
  size?: 'sm' | 'md' | 'lg';
  closable?: boolean;
  icon?: React.ReactNode;
  onClose?: () => void;
}

// Usage Examples
<Badge variant="success" count={5}>
  <Button>Messages</Button>
</Badge>

<Tag variant="primary" closable onClose={handleRemove}>
  JavaScript
</Tag>
```

---

## 7. Navigation Components

### 7.1 Breadcrumb Component

```typescript
// Breadcrumb Component Specification
interface BreadcrumbItem {
  title: string;
  href?: string;
  icon?: React.ReactNode;
  onClick?: () => void;
}

interface BreadcrumbProps extends BaseComponentProps {
  items: BreadcrumbItem[];
  separator?: React.ReactNode;
  maxItems?: number;
}

// Usage Examples
<Breadcrumb
  items={[
    { title: 'Home', href: '/' },
    { title: 'Social', href: '/social' },
    { title: 'Parties', href: '/social/parties' },
    { title: 'Study Group Alpha' },
  ]}
  separator={<ChevronRightIcon />}
/>
```

### 7.2 Tabs Component

```typescript
// Tabs Component Specification
interface TabItem {
  key: string;
  title: string;
  icon?: React.ReactNode;
  disabled?: boolean;
  badge?: number;
}

interface TabsProps extends BaseComponentProps {
  items: TabItem[];
  activeKey?: string;
  defaultActiveKey?: string;
  variant?: 'line' | 'card' | 'pill';
  size?: 'sm' | 'md' | 'lg';
  onChange?: (key: string) => void;
}

// Usage Examples
<Tabs
  items={[
    { key: 'overview', title: 'Overview', icon: <HomeIcon /> },
    { key: 'members', title: 'Members', badge: 12 },
    { key: 'settings', title: 'Settings' },
  ]}
  variant="line"
  onChange={setActiveTab}
/>
```

### 7.3 Pagination Component

```typescript
// Pagination Component Specification
interface PaginationProps extends BaseComponentProps {
  current: number;
  total: number;
  pageSize?: number;
  showSizeChanger?: boolean;
  showQuickJumper?: boolean;
  showTotal?: (total: number, range: [number, number]) => string;
  onChange?: (page: number, pageSize: number) => void;
}

// Usage Examples
<Pagination
  current={currentPage}
  total={totalItems}
  pageSize={20}
  showSizeChanger
  showTotal={(total, range) =>
    `${range[0]}-${range[1]} of ${total} items`
  }
  onChange={handlePageChange}
/>
```

---

## 8. Feedback Components

### 8.1 Alert Component

```typescript
// Alert Component Specification
interface AlertProps extends BaseComponentProps {
  type?: 'info' | 'success' | 'warning' | 'error';
  title?: string;
  description?: string;
  closable?: boolean;
  showIcon?: boolean;
  actions?: React.ReactNode;
  onClose?: () => void;
}

// Usage Examples
<Alert
  type="success"
  title="Party Created Successfully"
  description="Your study party has been created and is ready for members to join."
  closable
  showIcon
/>

<Alert
  type="warning"
  title="Connection Issues"
  description="We're experiencing some connectivity issues. Please try again."
  actions={
    <Button variant="outline" size="sm">
      Retry
    </Button>
  }
/>
```

### 8.2 Toast/Notification Component

```typescript
// Toast Component Specification
interface ToastProps extends BaseComponentProps {
  type?: 'info' | 'success' | 'warning' | 'error';
  title: string;
  description?: string;
  duration?: number;
  closable?: boolean;
  actions?: React.ReactNode;
  onClose?: () => void;
}

// Toast Manager
interface ToastManager {
  show: (toast: Omit<ToastProps, 'onClose'>) => string;
  hide: (id: string) => void;
  hideAll: () => void;
}

// Usage Examples
toast.show({
  type: 'success',
  title: 'Member Added',
  description: 'John Doe has been added to your party.',
  duration: 5000,
});

toast.show({
  type: 'error',
  title: 'Failed to Save',
  description: 'Please check your connection and try again.',
  actions: <Button variant="outline" size="sm">Retry</Button>,
});
```

### 8.3 Loading Components

```typescript
// Spinner Component Specification
interface SpinnerProps extends BaseComponentProps {
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl';
  color?: string;
}

// Skeleton Component Specification
interface SkeletonProps extends BaseComponentProps {
  width?: string | number;
  height?: string | number;
  variant?: 'text' | 'rectangular' | 'circular';
  animation?: 'pulse' | 'wave' | 'none';
}

// Progress Component Specification
interface ProgressProps extends BaseComponentProps {
  value: number;
  max?: number;
  size?: 'sm' | 'md' | 'lg';
  variant?: 'linear' | 'circular';
  showLabel?: boolean;
  color?: string;
}

// Usage Examples
<Spinner size="md" />

<Skeleton variant="text" width="100%" height={20} />
<Skeleton variant="rectangular" width={200} height={100} />
<Skeleton variant="circular" width={40} height={40} />

<Progress value={75} max={100} showLabel />
```

---

## 9. Feature-Specific Components

### 9.1 Party Management Components

```typescript
// Party Card Component
interface PartyCardProps extends BaseComponentProps {
  party: Party;
  currentUser: User;
  onJoin?: (partyId: string) => void;
  onLeave?: (partyId: string) => void;
  onEdit?: (partyId: string) => void;
}

// Member List Component
interface MemberListProps extends BaseComponentProps {
  members: PartyMember[];
  currentUserRole: MemberRole;
  onRoleChange?: (memberId: string, role: MemberRole) => void;
  onRemove?: (memberId: string) => void;
}

// Meeting Scheduler Component
interface MeetingSchedulerProps extends BaseComponentProps {
  partyId: string;
  availableMembers: PartyMember[];
  onSchedule?: (meeting: MeetingData) => void;
}
```

### 9.2 Guild Management Components

```typescript
// Guild Analytics Dashboard
interface GuildAnalyticsDashboardProps extends BaseComponentProps {
  guild: Guild;
  analytics: GuildAnalytics;
  timeRange: TimeRange;
  onTimeRangeChange?: (range: TimeRange) => void;
}

// Member Progress Chart
interface MemberProgressChartProps extends BaseComponentProps {
  members: GuildMember[];
  metric: ProgressMetric;
  timeRange: TimeRange;
}

// Guild Settings Panel
interface GuildSettingsPanelProps extends BaseComponentProps {
  guild: Guild;
  onUpdate?: (updates: Partial<Guild>) => void;
}
```

### 9.3 Code Battle Components

```typescript
// Tournament Bracket
interface TournamentBracketProps extends BaseComponentProps {
  tournament: Tournament;
  matches: Match[];
  onMatchClick?: (matchId: string) => void;
}

// Code Editor
interface CodeEditorProps extends BaseComponentProps {
  language: string;
  value: string;
  readOnly?: boolean;
  theme?: 'light' | 'dark';
  onChange?: (value: string) => void;
  onSubmit?: (code: string) => void;
}

// Battle Leaderboard
interface BattleLeaderboardProps extends BaseComponentProps {
  participants: BattleParticipant[];
  currentUser: User;
  realTime?: boolean;
}
```

### 9.4 Collaboration Components

```typescript
// Video Conference
interface VideoConferenceProps extends BaseComponentProps {
  roomId: string;
  participants: Participant[];
  localStream?: MediaStream;
  onStreamChange?: (stream: MediaStream) => void;
}

// Collaborative Editor
interface CollaborativeEditorProps extends BaseComponentProps {
  documentId: string;
  initialContent: string;
  participants: Participant[];
  readOnly?: boolean;
  onContentChange?: (content: string) => void;
}

// Shared Whiteboard
interface SharedWhiteboardProps extends BaseComponentProps {
  boardId: string;
  tools: WhiteboardTool[];
  readOnly?: boolean;
  onDrawingChange?: (drawing: DrawingData) => void;
}
```

---

## 10. Component Usage Guidelines

### 10.1 Accessibility Standards

**WCAG 2.1 AA Compliance:**
- ✅ Color contrast ratio ≥ 4.5:1 for normal text
- ✅ Color contrast ratio ≥ 3:1 for large text
- ✅ Focus indicators visible and consistent
- ✅ Keyboard navigation support
- ✅ Screen reader compatibility
- ✅ Alternative text for images
- ✅ Semantic HTML structure

```typescript
// Accessibility Props Interface
interface AccessibilityProps {
  'aria-label'?: string;
  'aria-labelledby'?: string;
  'aria-describedby'?: string;
  'aria-expanded'?: boolean;
  'aria-selected'?: boolean;
  'aria-disabled'?: boolean;
  'aria-hidden'?: boolean;
  role?: string;
  tabIndex?: number;
}
```

### 10.2 Responsive Design Guidelines

**Breakpoint Strategy:**
```typescript
// Responsive Breakpoints
const breakpoints = {
  xs: '320px',  // Mobile portrait
  sm: '640px',  // Mobile landscape
  md: '768px',  // Tablet portrait
  lg: '1024px', // Tablet landscape / Small desktop
  xl: '1280px', // Desktop
  '2xl': '1536px', // Large desktop
};

// Responsive Component Example
const ResponsiveComponent = () => {
  const { isMobile, isTablet, isDesktop } = useResponsive();
  
  return (
    <div className={cn(
      'grid gap-4',
      isMobile && 'grid-cols-1',
      isTablet && 'grid-cols-2',
      isDesktop && 'grid-cols-3'
    )}>
      {/* Content */}
    </div>
  );
};
```

### 10.3 Performance Optimization

**Component Optimization Strategies:**
```typescript
// Memoization for expensive components
const ExpensiveComponent = React.memo(({ data, onUpdate }) => {
  const processedData = useMemo(() => {
    return data.map(item => processItem(item));
  }, [data]);
  
  const handleUpdate = useCallback((id, updates) => {
    onUpdate(id, updates);
  }, [onUpdate]);
  
  return (
    <div>
      {processedData.map(item => (
        <Item
          key={item.id}
          data={item}
          onUpdate={handleUpdate}
        />
      ))}
    </div>
  );
});

// Lazy loading for heavy components
const HeavyComponent = lazy(() => import('./HeavyComponent'));

// Virtual scrolling for large lists
const VirtualizedList = ({ items }) => {
  return (
    <FixedSizeList
      height={400}
      itemCount={items.length}
      itemSize={50}
      itemData={items}
    >
      {ListItem}
    </FixedSizeList>
  );
};
```

### 10.4 Testing Guidelines

**Component Testing Strategy:**
```typescript
// Unit Test Example
describe('Button Component', () => {
  it('renders with correct variant styles', () => {
    render(<Button variant="primary">Click me</Button>);
    
    const button = screen.getByRole('button');
    expect(button).toHaveClass('bg-primary-500');
  });
  
  it('handles click events', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
  
  it('is accessible', async () => {
    const { container } = render(<Button>Click me</Button>);
    const results = await axe(container);
    expect(results).toHaveNoViolations();
  });
});

// Integration Test Example
describe('Party Creation Flow', () => {
  it('creates party successfully', async () => {
    render(<PartyCreationWizard />);
    
    // Fill form steps
    await userEvent.type(screen.getByLabelText('Party Name'), 'Test Party');
    await userEvent.click(screen.getByText('Next'));
    
    // Submit form
    await userEvent.click(screen.getByText('Create Party'));
    
    // Verify success
    expect(await screen.findByText('Party created successfully')).toBeInTheDocument();
  });
});
```

### 10.5 Documentation Standards

**Component Documentation Template:**
```typescript
/**
 * Button Component
 * 
 * A versatile button component that supports multiple variants, sizes, and states.
 * 
 * @example
 * ```tsx
 * <Button variant="primary" size="md" onClick={handleClick}>
 *   Click me
 * </Button>
 * ```
 * 
 * @param variant - Visual style variant
 * @param size - Button size
 * @param disabled - Whether the button is disabled
 * @param loading - Whether to show loading state
 * @param onClick - Click event handler
 */
export const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'md',
  disabled = false,
  loading = false,
  onClick,
  children,
  ...props
}) => {
  // Component implementation
};
```

---

## Implementation Checklist

### Phase 1: Foundation Components ✅
- [x] Design tokens and theme system
- [x] Base component interfaces
- [x] Button, Input, Card components
- [x] Layout components (Container, Grid, Stack)

### Phase 2: Form & Data Components 🔄
- [ ] Form field components
- [ ] Select, Checkbox, Radio components
- [ ] Table and List components
- [ ] Badge and Tag components

### Phase 3: Navigation & Feedback 📋
- [ ] Breadcrumb and Tabs components
- [ ] Pagination component
- [ ] Alert and Toast components
- [ ] Loading components (Spinner, Skeleton, Progress)

### Phase 4: Feature-Specific Components 📋
- [ ] Party management components
- [ ] Guild management components
- [ ] Code battle components
- [ ] Collaboration components

### Phase 5: Testing & Documentation 📋
- [ ] Unit tests for all components
- [ ] Accessibility testing
- [ ] Storybook documentation
- [ ] Usage guidelines and examples

---

*This component library specification provides a comprehensive foundation for building consistent, accessible, and performant UI components for the RogueLearn platform.*