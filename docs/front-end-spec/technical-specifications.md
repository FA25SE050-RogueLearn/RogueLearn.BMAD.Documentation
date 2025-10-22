# RogueLearn Technical Specifications
## Phase 2 & Phase 3 Frontend Implementation Guide

---

## Table of Contents

1. [Architecture Overview](#1-architecture-overview)
2. [Technology Stack](#2-technology-stack)
3. [Component Specifications](#3-component-specifications)
4. [API Integration Requirements](#4-api-integration-requirements)
5. [Real-time Features Implementation](#5-real-time-features-implementation)
6. [Mobile Responsiveness](#6-mobile-responsiveness)
7. [Performance Requirements](#7-performance-requirements)
8. [Security Considerations](#8-security-considerations)
9. [Testing Strategy](#9-testing-strategy)
10. [Deployment & DevOps](#10-deployment--devops)

---

## 1. Architecture Overview

### 1.1 Frontend Architecture Pattern

**Recommended Architecture**: **Micro-Frontend with Module Federation**

```
┌─────────────────────────────────────────────────────────────┐
│                    Shell Application                        │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │   Core UI   │  │   Social    │  │   Guild     │         │
│  │  Components │  │  Features   │  │ Management  │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│                                                             │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │    Event    │  │   Events    │  │  Real-time  │         │
│  │ Competition │  │ Management  │  │Collaboration│         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 State Management Architecture

**Primary**: **Redux Toolkit + RTK Query**
**Real-time**: **WebSocket + Redux Middleware**

```typescript
// Store Structure
interface RootState {
  auth: AuthState;
  user: UserState;
  social: SocialState;
  guilds: GuildState;
  battles: BattleState;
  events: EventState;
  collaboration: CollaborationState;
  ui: UIState;
  realtime: RealtimeState;
}
```

### 1.3 Routing Strategy

**Router**: **React Router v6 with Lazy Loading**

```typescript
// Route Structure
const routes = [
  {
    path: "/",
    element: <Layout />,
    children: [
      { path: "dashboard", element: lazy(() => import("./Dashboard")) },
      { path: "social/*", element: lazy(() => import("./Social")) },
      { path: "guilds/*", element: lazy(() => import("./Guilds")) },
      { path: "battles/*", element: lazy(() => import("./Battles")) },
      { path: "events/*", element: lazy(() => import("./Events")) },
      { path: "collaboration/*", element: lazy(() => import("./Collaboration")) }
    ]
  }
];
```

---

## 2. Technology Stack

### 2.1 Core Technologies

| Category | Technology | Version | Purpose |
|----------|------------|---------|---------|
| **Framework** | React | 18.2+ | UI Framework |
| **Language** | TypeScript | 5.0+ | Type Safety |
| **Build Tool** | Vite | 4.0+ | Development & Build |
| **State Management** | Redux Toolkit | 1.9+ | Global State |
| **Data Fetching** | RTK Query | 1.9+ | API Management |
| **Routing** | React Router | 6.8+ | Client-side Routing |
| **Styling** | Tailwind CSS | 3.3+ | Utility-first CSS |
| **UI Components** | Headless UI | 1.7+ | Accessible Components |
| **Icons** | Heroicons | 2.0+ | Icon Library |
| **Forms** | React Hook Form | 7.43+ | Form Management |
| **Validation** | Zod | 3.20+ | Schema Validation |

### 2.2 Real-time Technologies

| Technology | Purpose | Implementation |
|------------|---------|----------------|
| **Socket.IO** | Real-time Communication | WebSocket connections |
| **WebRTC** | Video/Audio Calls | Peer-to-peer communication |
| **Yjs** | Collaborative Editing | CRDT for real-time sync |
| **Monaco Editor** | Code Editing | VS Code editor integration |

### 2.3 Development Tools

| Tool | Purpose |
|------|---------|
| **ESLint** | Code Linting |
| **Prettier** | Code Formatting |
| **Husky** | Git Hooks |
| **Jest** | Unit Testing |
| **Cypress** | E2E Testing |
| **Storybook** | Component Documentation |

---

## 3. Component Specifications

### 3.1 Core Component Library

#### 3.1.1 Layout Components

```typescript
// Header Component
interface HeaderProps {
  user: User;
  notifications: Notification[];
  onNotificationClick: (id: string) => void;
  onProfileClick: () => void;
}

// Sidebar Component
interface SidebarProps {
  currentPath: string;
  userRole: UserRole;
  guildMemberships: Guild[];
  onNavigate: (path: string) => void;
}

// Breadcrumb Component
interface BreadcrumbProps {
  items: BreadcrumbItem[];
  separator?: React.ReactNode;
}
```

#### 3.1.2 Form Components

```typescript
// Form Field Component
interface FormFieldProps {
  label: string;
  name: string;
  type: 'text' | 'email' | 'password' | 'select' | 'textarea' | 'file';
  validation?: ZodSchema;
  placeholder?: string;
  options?: SelectOption[];
  required?: boolean;
  disabled?: boolean;
}

// Multi-step Form Component
interface MultiStepFormProps {
  steps: FormStep[];
  currentStep: number;
  onStepChange: (step: number) => void;
  onSubmit: (data: any) => Promise<void>;
  validationSchema: ZodSchema;
}
```

#### 3.1.3 Data Display Components

```typescript
// Data Table Component
interface DataTableProps<T> {
  data: T[];
  columns: TableColumn<T>[];
  pagination?: PaginationConfig;
  sorting?: SortingConfig;
  filtering?: FilterConfig;
  selection?: SelectionConfig;
  loading?: boolean;
  onRowClick?: (row: T) => void;
}

// Card Component
interface CardProps {
  title?: string;
  subtitle?: string;
  actions?: React.ReactNode;
  children: React.ReactNode;
  variant?: 'default' | 'outlined' | 'elevated';
  size?: 'sm' | 'md' | 'lg';
}
```

### 3.2 Feature-Specific Components

#### 3.2.1 Social Features

```typescript
// Party Creation Wizard
interface PartyCreationWizardProps {
  onComplete: (partyData: PartyCreationData) => Promise<void>;
  initialData?: Partial<PartyCreationData>;
}

// Member Management Component
interface MemberManagementProps {
  members: PartyMember[];
  currentUserRole: MemberRole;
  onRoleChange: (memberId: string, role: MemberRole) => Promise<void>;
  onRemoveMember: (memberId: string) => Promise<void>;
  onInviteMember: (email: string) => Promise<void>;
}
```

#### 3.2.2 Guild Management

```typescript
// Guild Dashboard Component
interface GuildDashboardProps {
  guild: Guild;
  analytics: GuildAnalytics;
  recentActivity: Activity[];
  onUpdateGuild: (updates: Partial<Guild>) => Promise<void>;
}

// Member Progress Tracker
interface MemberProgressTrackerProps {
  members: GuildMember[];
  timeRange: TimeRange;
  metrics: ProgressMetric[];
  onExportData: () => void;
}
```

#### 3.2.3 Event System

```typescript
// Tournament Bracket Component
interface TournamentBracketProps {
  tournament: Tournament;
  matches: Match[];
  currentUser: User;
  onMatchClick: (matchId: string) => void;
}

// Live Event Interface
interface LiveEventInterfaceProps {
  event: Event;
  participants: Participant[];
  problem: Problem;
  isSpectator: boolean;
  onSubmitCode?: (code: string) => Promise<void>;
}
```

#### 3.2.4 Real-time Collaboration

```typescript
// Collaborative Editor Component
interface CollaborativeEditorProps {
  documentId: string;
  initialContent: string;
  participants: Participant[];
  onContentChange: (content: string) => void;
  readOnly?: boolean;
}

// Video Conference Component
interface VideoConferenceProps {
  roomId: string;
  participants: Participant[];
  localStream?: MediaStream;
  onStreamChange: (stream: MediaStream) => void;
  onParticipantJoin: (participant: Participant) => void;
  onParticipantLeave: (participantId: string) => void;
}
```

---

## 4. API Integration Requirements

### 4.1 API Client Configuration

```typescript
// RTK Query API Configuration
export const api = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({
    baseUrl: '/api/v1',
    prepareHeaders: (headers, { getState }) => {
      const token = (getState() as RootState).auth.token;
      if (token) {
        headers.set('authorization', `Bearer ${token}`);
      }
      return headers;
    },
  }),
  tagTypes: ['User', 'Party', 'Guild', 'Battle', 'Event', 'Collaboration'],
  endpoints: (builder) => ({
    // Endpoints will be injected by feature modules
  }),
});
```

### 4.2 API Endpoints Specification

#### 4.2.1 Social Features APIs

```typescript
// Party Management APIs
export const partyApi = api.injectEndpoints({
  endpoints: (builder) => ({
    createParty: builder.mutation<Party, CreatePartyRequest>({
      query: (data) => ({
        url: '/parties',
        method: 'POST',
        body: data,
      }),
      invalidatesTags: ['Party'],
    }),
    
    getParties: builder.query<Party[], GetPartiesRequest>({
      query: (params) => ({
        url: '/parties',
        params,
      }),
      providesTags: ['Party'],
    }),
    
    updateParty: builder.mutation<Party, UpdatePartyRequest>({
      query: ({ id, ...data }) => ({
        url: `/parties/${id}`,
        method: 'PATCH',
        body: data,
      }),
      invalidatesTags: ['Party'],
    }),
  }),
});
```

#### 4.2.2 Guild Management APIs

```typescript
// Guild Management APIs
export const guildApi = api.injectEndpoints({
  endpoints: (builder) => ({
    createGuild: builder.mutation<Guild, CreateGuildRequest>({
      query: (data) => ({
        url: '/guilds',
        method: 'POST',
        body: data,
      }),
      invalidatesTags: ['Guild'],
    }),
    
    getGuildAnalytics: builder.query<GuildAnalytics, string>({
      query: (guildId) => `/guilds/${guildId}/analytics`,
      providesTags: ['Guild'],
    }),
    
    manageGuildMember: builder.mutation<void, ManageMemberRequest>({
      query: ({ guildId, memberId, action, data }) => ({
        url: `/guilds/${guildId}/members/${memberId}/${action}`,
        method: 'POST',
        body: data,
      }),
      invalidatesTags: ['Guild'],
    }),
  }),
});
```

#### 4.2.3 Code Battle APIs

```typescript
// Code Battle APIs
export const battleApi = api.injectEndpoints({
  endpoints: (builder) => ({
    createBattle: builder.mutation<Battle, CreateBattleRequest>({
      query: (data) => ({
        url: '/battles',
        method: 'POST',
        body: data,
      }),
      invalidatesTags: ['Event'],
    }),
    
    joinEvent: builder.mutation<void, JoinEventRequest>({
      query: ({ eventId, ...data }) => ({
        url: `/events/${eventId}/join`,
        method: 'POST',
        body: data,
      }),
      invalidatesTags: ['Event'],
    }),
    
    submitCode: builder.mutation<SubmissionResult, SubmitCodeRequest>({
      query: ({ eventId, ...data }) => ({
        url: `/events/${eventId}/submit`,
        method: 'POST',
        body: data,
      }),
    }),
  }),
});
```

### 4.3 Error Handling Strategy

```typescript
// Global Error Handling Middleware
const errorHandlingMiddleware: Middleware = (store) => (next) => (action) => {
  if (action.type.endsWith('/rejected')) {
    const error = action.payload;
    
    // Handle different error types
    switch (error.status) {
      case 401:
        store.dispatch(logout());
        break;
      case 403:
        store.dispatch(showNotification({
          type: 'error',
          message: 'Access denied',
        }));
        break;
      case 500:
        store.dispatch(showNotification({
          type: 'error',
          message: 'Server error. Please try again later.',
        }));
        break;
      default:
        store.dispatch(showNotification({
          type: 'error',
          message: error.message || 'An unexpected error occurred',
        }));
    }
  }
  
  return next(action);
};
```

---

## 5. Real-time Features Implementation

### 5.1 WebSocket Connection Management

```typescript
// WebSocket Manager
class WebSocketManager {
  private socket: Socket | null = null;
  private reconnectAttempts = 0;
  private maxReconnectAttempts = 5;
  
  connect(token: string) {
    this.socket = io(WS_URL, {
      auth: { token },
      transports: ['websocket'],
    });
    
    this.setupEventHandlers();
  }
  
  private setupEventHandlers() {
    if (!this.socket) return;
    
    this.socket.on('connect', () => {
      console.log('WebSocket connected');
      this.reconnectAttempts = 0;
    });
    
    this.socket.on('disconnect', () => {
      console.log('WebSocket disconnected');
      this.handleReconnect();
    });
    
    // Feature-specific event handlers
    this.socket.on('party:update', (data) => {
      store.dispatch(partyUpdated(data));
    });
    
    this.socket.on('event:update', (data) => {
      store.dispatch(eventUpdated(data));
    });
    
    this.socket.on('collaboration:change', (data) => {
      store.dispatch(collaborationChanged(data));
    });
  }
}
```

### 5.2 Real-time Collaboration Implementation

```typescript
// Collaborative Document Manager
class CollaborativeDocumentManager {
  private yDoc: Y.Doc;
  private provider: WebsocketProvider;
  private yText: Y.Text;
  
  constructor(documentId: string, token: string) {
    this.yDoc = new Y.Doc();
    this.provider = new WebsocketProvider(
      WS_URL,
      documentId,
      this.yDoc,
      { params: { token } }
    );
    this.yText = this.yDoc.getText('content');
  }
  
  bindToEditor(editor: monaco.editor.IStandaloneCodeEditor) {
    const monacoBinding = new MonacoBinding(
      this.yText,
      editor.getModel()!,
      new Set([editor]),
      this.provider.awareness
    );
    
    return monacoBinding;
  }
  
  getCursors(): Map<string, any> {
    return this.provider.awareness.getStates();
  }
}
```

### 5.3 Video Conference Integration

```typescript
// WebRTC Manager
class WebRTCManager {
  private localStream: MediaStream | null = null;
  private peerConnections: Map<string, RTCPeerConnection> = new Map();
  private socket: Socket;
  
  constructor(socket: Socket) {
    this.socket = socket;
    this.setupSocketHandlers();
  }
  
  async startCall(roomId: string): Promise<MediaStream> {
    this.localStream = await navigator.mediaDevices.getUserMedia({
      video: true,
      audio: true,
    });
    
    this.socket.emit('join-room', roomId);
    return this.localStream;
  }
  
  private async createPeerConnection(userId: string): Promise<RTCPeerConnection> {
    const pc = new RTCPeerConnection({
      iceServers: [{ urls: 'stun:stun.l.google.com:19302' }],
    });
    
    // Add local stream tracks
    if (this.localStream) {
      this.localStream.getTracks().forEach(track => {
        pc.addTrack(track, this.localStream!);
      });
    }
    
    // Handle remote stream
    pc.ontrack = (event) => {
      const remoteStream = event.streams[0];
      this.handleRemoteStream(userId, remoteStream);
    };
    
    this.peerConnections.set(userId, pc);
    return pc;
  }
}
```

---

## 6. Mobile Responsiveness

### 6.1 Responsive Breakpoints

```css
/* Tailwind CSS Custom Breakpoints */
module.exports = {
  theme: {
    screens: {
      'xs': '320px',
      'sm': '640px',
      'md': '768px',
      'lg': '1024px',
      'xl': '1280px',
      '2xl': '1536px',
    },
  },
}
```

### 6.2 Mobile-First Component Design

```typescript
// Responsive Hook
const useResponsive = () => {
  const [screenSize, setScreenSize] = useState<ScreenSize>('lg');
  
  useEffect(() => {
    const handleResize = () => {
      const width = window.innerWidth;
      if (width < 640) setScreenSize('xs');
      else if (width < 768) setScreenSize('sm');
      else if (width < 1024) setScreenSize('md');
      else if (width < 1280) setScreenSize('lg');
      else setScreenSize('xl');
    };
    
    handleResize();
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return {
    screenSize,
    isMobile: screenSize === 'xs' || screenSize === 'sm',
    isTablet: screenSize === 'md',
    isDesktop: screenSize === 'lg' || screenSize === 'xl',
  };
};
```

### 6.3 Touch-Optimized Components

```typescript
// Touch-friendly Button Component
interface TouchButtonProps extends ButtonProps {
  touchTarget?: 'small' | 'medium' | 'large';
}

const TouchButton: React.FC<TouchButtonProps> = ({
  touchTarget = 'medium',
  className,
  ...props
}) => {
  const touchClasses = {
    small: 'min-h-[36px] min-w-[36px]',
    medium: 'min-h-[44px] min-w-[44px]',
    large: 'min-h-[56px] min-w-[56px]',
  };
  
  return (
    <button
      className={cn(
        touchClasses[touchTarget],
        'touch-manipulation',
        className
      )}
      {...props}
    />
  );
};
```

---

## 7. Performance Requirements

### 7.1 Loading Performance Targets

| Metric | Target | Measurement |
|--------|--------|-------------|
| **First Contentful Paint** | < 1.5s | Core Web Vitals |
| **Largest Contentful Paint** | < 2.5s | Core Web Vitals |
| **Cumulative Layout Shift** | < 0.1 | Core Web Vitals |
| **First Input Delay** | < 100ms | Core Web Vitals |
| **Time to Interactive** | < 3s | Lighthouse |
| **Bundle Size** | < 500KB (gzipped) | Webpack Bundle Analyzer |

### 7.2 Code Splitting Strategy

```typescript
// Route-based Code Splitting
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Social = lazy(() => import('./pages/Social'));
const Guilds = lazy(() => import('./pages/Guilds'));
const Battles = lazy(() => import('./pages/Battles'));

// Component-based Code Splitting
const HeavyComponent = lazy(() => import('./components/HeavyComponent'));

// Feature-based Code Splitting
const CodeEditor = lazy(() => 
  import('./components/CodeEditor').then(module => ({
    default: module.CodeEditor
  }))
);
```

### 7.3 Caching Strategy

```typescript
// Service Worker Configuration
const CACHE_NAME = 'roguelearn-v1';
const STATIC_ASSETS = [
  '/',
  '/static/css/main.css',
  '/static/js/main.js',
  '/manifest.json',
];

// Cache Strategy
const cacheStrategy = {
  // Static assets - Cache First
  static: new CacheFirst({
    cacheName: 'static-assets',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 100,
        maxAgeSeconds: 30 * 24 * 60 * 60, // 30 days
      }),
    ],
  }),
  
  // API calls - Network First
  api: new NetworkFirst({
    cacheName: 'api-cache',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 50,
        maxAgeSeconds: 5 * 60, // 5 minutes
      }),
    ],
  }),
  
  // Images - Cache First
  images: new CacheFirst({
    cacheName: 'images',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 200,
        maxAgeSeconds: 7 * 24 * 60 * 60, // 7 days
      }),
    ],
  }),
};
```

---

## 8. Security Considerations

### 8.1 Authentication & Authorization

```typescript
// JWT Token Management
class TokenManager {
  private static readonly ACCESS_TOKEN_KEY = 'access_token';
  private static readonly REFRESH_TOKEN_KEY = 'refresh_token';
  
  static setTokens(accessToken: string, refreshToken: string) {
    // Store in httpOnly cookies for security
    document.cookie = `${this.ACCESS_TOKEN_KEY}=${accessToken}; HttpOnly; Secure; SameSite=Strict`;
    document.cookie = `${this.REFRESH_TOKEN_KEY}=${refreshToken}; HttpOnly; Secure; SameSite=Strict`;
  }
  
  static async refreshToken(): Promise<string | null> {
    try {
      const response = await fetch('/api/auth/refresh', {
        method: 'POST',
        credentials: 'include',
      });
      
      if (response.ok) {
        const { accessToken } = await response.json();
        return accessToken;
      }
    } catch (error) {
      console.error('Token refresh failed:', error);
    }
    
    return null;
  }
}
```

### 8.2 Input Validation & Sanitization

```typescript
// Input Validation Schemas
export const partyCreationSchema = z.object({
  name: z.string()
    .min(3, 'Name must be at least 3 characters')
    .max(50, 'Name must not exceed 50 characters')
    .regex(/^[a-zA-Z0-9\s-_]+$/, 'Name contains invalid characters'),
  
  description: z.string()
    .max(500, 'Description must not exceed 500 characters')
    .optional(),
  
  maxMembers: z.number()
    .min(2, 'Minimum 2 members required')
    .max(20, 'Maximum 20 members allowed'),
  
  isPrivate: z.boolean(),
  
  tags: z.array(z.string())
    .max(5, 'Maximum 5 tags allowed'),
});

// XSS Prevention
const sanitizeHtml = (html: string): string => {
  return DOMPurify.sanitize(html, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'a', 'p', 'br'],
    ALLOWED_ATTR: ['href'],
  });
};
```

### 8.3 Content Security Policy

```typescript
// CSP Configuration
const cspDirectives = {
  'default-src': ["'self'"],
  'script-src': ["'self'", "'unsafe-inline'", 'https://cdn.jsdelivr.net'],
  'style-src': ["'self'", "'unsafe-inline'", 'https://fonts.googleapis.com'],
  'font-src': ["'self'", 'https://fonts.gstatic.com'],
  'img-src': ["'self'", 'data:', 'https:'],
  'connect-src': ["'self'", 'wss:', 'https:'],
  'media-src': ["'self'"],
  'object-src': ["'none'"],
  'base-uri': ["'self'"],
  'form-action': ["'self'"],
  'frame-ancestors': ["'none'"],
  'upgrade-insecure-requests': [],
};
```

---

## 9. Testing Strategy

### 9.1 Testing Pyramid

```
┌─────────────────────────────────────┐
│           E2E Tests (10%)           │  ← Cypress
├─────────────────────────────────────┤
│       Integration Tests (20%)       │  ← React Testing Library
├─────────────────────────────────────┤
│         Unit Tests (70%)            │  ← Jest + RTL
└─────────────────────────────────────┘
```

### 9.2 Unit Testing Configuration

```typescript
// Jest Configuration
export default {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/src/test/setup.ts'],
  moduleNameMapping: {
    '^@/(.*)$': '<rootDir>/src/$1',
    '\\.(css|less|scss|sass)$': 'identity-obj-proxy',
  },
  collectCoverageFrom: [
    'src/**/*.{ts,tsx}',
    '!src/**/*.d.ts',
    '!src/test/**',
    '!src/stories/**',
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};

// Test Utilities
export const renderWithProviders = (
  ui: React.ReactElement,
  options: RenderOptions = {}
) => {
  const store = setupStore();
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false },
    },
  });
  
  const Wrapper = ({ children }: { children: React.ReactNode }) => (
    <Provider store={store}>
      <QueryClientProvider client={queryClient}>
        <BrowserRouter>
          {children}
        </BrowserRouter>
      </QueryClientProvider>
    </Provider>
  );
  
  return render(ui, { wrapper: Wrapper, ...options });
};
```

### 9.3 E2E Testing Strategy

```typescript
// Cypress Configuration
describe('Party Management Flow', () => {
  beforeEach(() => {
    cy.login('test@example.com', 'password');
    cy.visit('/social/parties');
  });
  
  it('should create a new party successfully', () => {
    cy.get('[data-testid="create-party-btn"]').click();
    
    // Step 1: Basic Information
    cy.get('[data-testid="party-name"]').type('Test Study Party');
    cy.get('[data-testid="party-description"]').type('A test party for studying');
    cy.get('[data-testid="next-step-btn"]').click();
    
    // Step 2: Configuration
    cy.get('[data-testid="max-members"]').clear().type('10');
    cy.get('[data-testid="privacy-toggle"]').click();
    cy.get('[data-testid="next-step-btn"]').click();
    
    // Step 3: Schedule
    cy.get('[data-testid="schedule-type"]').select('recurring');
    cy.get('[data-testid="create-party-submit"]').click();
    
    // Verify creation
    cy.get('[data-testid="success-message"]').should('be.visible');
    cy.url().should('include', '/social/parties/');
  });
});
```

---

## 10. Deployment & DevOps

### 10.1 Build Configuration

```typescript
// Vite Production Configuration
export default defineConfig({
  build: {
    target: 'es2020',
    outDir: 'dist',
    sourcemap: true,
    minify: 'terser',
    terserOptions: {
      compress: {
        drop_console: true,
        drop_debugger: true,
      },
    },
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          router: ['react-router-dom'],
          ui: ['@headlessui/react', '@heroicons/react'],
          editor: ['monaco-editor'],
          charts: ['recharts'],
        },
      },
    },
  },
  define: {
    __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
    __BUILD_TIME__: JSON.stringify(new Date().toISOString()),
  },
});
```

### 10.2 Environment Configuration

```typescript
// Environment Variables
interface EnvironmentConfig {
  NODE_ENV: 'development' | 'staging' | 'production';
  VITE_API_BASE_URL: string;
  VITE_WS_URL: string;
  VITE_SENTRY_DSN?: string;
  VITE_ANALYTICS_ID?: string;
  VITE_FEATURE_FLAGS?: string;
}

// Feature Flags
export const featureFlags = {
  enableCodeBattles: process.env.VITE_ENABLE_CODE_BATTLES === 'true',
  enableVideoConference: process.env.VITE_ENABLE_VIDEO_CONFERENCE === 'true',
  enableAdvancedAnalytics: process.env.VITE_ENABLE_ADVANCED_ANALYTICS === 'true',
};
```

### 10.3 CI/CD Pipeline

```yaml
# GitHub Actions Workflow
name: Frontend CI/CD

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check
      - run: npm run test:coverage
      - run: npm run build
      
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v3

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      - run: npm ci
      - run: npm run build
      
      - name: Deploy to Production
        run: |
          # Deploy to CDN/hosting service
          npm run deploy:prod
```

---

## Implementation Roadmap

### Phase 1: Foundation (Weeks 1-2)
- ✅ Set up development environment and tooling
- ✅ Implement core component library
- ✅ Set up state management and routing
- ✅ Implement authentication system

### Phase 2: Social Features (Weeks 3-4)
- 🔄 Implement party creation and management
- 🔄 Build meeting scheduler and communication tools
- 🔄 Add real-time messaging and notifications

### Phase 3: Guild Management (Weeks 5-6)
- 📋 Build guild dashboard and analytics
- 📋 Implement member management system
- 📋 Add progress tracking and reporting

### Phase 4: Code Battles (Weeks 7-8)
- 📋 Implement tournament system
- 📋 Build live coding interface
- 📋 Add spectator mode and analytics

### Phase 5: Advanced Features (Weeks 9-10)
- 📋 Implement real-time collaboration tools
- 📋 Add advanced social features
- 📋 Build event management system

### Phase 6: Polish & Optimization (Weeks 11-12)
- 📋 Performance optimization
- 📋 Mobile responsiveness refinement
- 📋 Testing and bug fixes
- 📋 Documentation and deployment

---

*This technical specification provides a comprehensive guide for implementing all RogueLearn Phase 2 and Phase 3 features with modern web technologies, ensuring scalability, performance, and maintainability.*