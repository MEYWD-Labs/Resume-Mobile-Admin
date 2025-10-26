# Resume-Mobile-Admin

**React Native Mobile Admin Application** for Resume Builder SaaS platform administrators.

## Overview

Resume-Mobile-Admin is a cross-platform mobile application built with React Native that provides administrators with comprehensive tools for managing users, subscriptions, support tickets, and system monitoring on the go. Features real-time alerts, offline capabilities, and secure admin operations.

## Technology Stack

- **Framework**: React Native 0.74+
- **Navigation**: React Navigation 6
- **State Management**: Zustand + React Query (TanStack Query)
- **UI Components**: React Native Elements / NativeBase
- **Styling**: StyleSheet + Styled Components
- **Forms**: React Hook Form + Yup validation
- **Storage**: AsyncStorage (offline), MMKV (performance)
- **Networking**: Axios + React Query
- **Real-time**: Socket.IO Client
- **Authentication**: JWT with biometric security
- **Charts**: React Native SVG Charts
- **Language**: TypeScript
- **Build Tools**: Expo CLI / Metro

## Key Features

### MVP-1: Core Admin Mobile Experience
- Admin authentication with MFA enforcement
- Dashboard with key metrics and alerts
- User search and basic management
- Real-time notifications and alerts
- Offline mode with sync
- Biometric security

### MVP-2: User & Subscription Management
- Advanced user search and filtering
- Subscription management (upgrade, downgrade, cancel)
- Payment history and refund processing
- User impersonation capabilities
- Bulk user operations

### MVP-3: Support & System Monitoring
- Support ticket management and response
- System health monitoring dashboard
- Real-time alerts and notifications
- Performance metrics visualization
- Log viewing and filtering

### MVP-4: Advanced Admin Features
- Feature flag management
- Template management and approval
- Analytics and reporting
- Team collaboration features
- Emergency maintenance tools

## Service Dependencies

**Depends On**:
- **Resume-SSO** - For admin authentication with MFA requirement
- **Resume-Admin-API** - For all admin operations and data

**No services depend on Resume-Mobile-Admin** - This is an administrative application.

## Application Architecture

### Navigation Structure
```
App
├── Auth Stack
│   ├── Login
│   ├── MFA Verification
│   └── Biometric Setup
├── Main Stack (Authenticated)
│   ├── Tab Navigator
│   │   ├── Dashboard Tab
│   │   ├── Users Tab
│   │   ├── Support Tab
│   │   └── System Tab
│   ├── User Management Stack
│   │   ├── User List
│   │   ├── User Details
│   │   ├── User Edit
│   │   └── User Activity
│   ├── Support Stack
│   │   ├── Ticket List
│   │   ├── Ticket Details
│   │   ├── Ticket Reply
│   │   └── Ticket Create
│   ├── System Stack
│   │   ├── Health Monitor
│   │   ├── Metrics Dashboard
│   │   ├── Log Viewer
│   │   └── Alerts Center
│   └── Settings Stack
│       ├── Admin Settings
│       ├── Notifications
│       └── Security
└── Modal Stack
    ├── Emergency Actions
    ├── User Impersonation
    └── System Alerts
```

## Component Architecture

### Core Components

#### Authentication Components
```typescript
// Admin auth components
- AdminLoginForm
- MFAVerificationScreen
- BiometricAuthSetup
- SecurityChallengeScreen
- SessionTimeoutWarning
```

#### Dashboard Components
```typescript
// Dashboard components
- MetricsOverview
- AlertBanner
- QuickActionsGrid
- RecentActivityList
- SystemHealthIndicator
- NotificationCenter
```

#### User Management Components
```typescript
// User management
- UserSearchBar
- UserListCard
- UserDetailScreen
- UserEditForm
- SubscriptionManager
- BulkOperationSelector
```

#### Support Components
```typescript
// Support ticket components
- TicketListItem
- TicketDetailScreen
- ReplyComposer
- TicketStatusBadge
- PriorityIndicator
- AssignmentSelector
```

#### System Monitoring Components
```typescript
// System monitoring
- HealthStatusCard
- MetricsChart
- LogViewer
- AlertListItem
- PerformanceIndicator
- SystemMetricsGrid
```

## Screen-by-Screen UX Design

### Authentication Screens

#### Admin Login Screen
- **Layout**: Secure login with branding
- **Fields**: Email, password, MFA code field
- **Actions**: Login, Biometric option, Emergency access
- **Features**: MFA enforcement, Session timeout warning
- **Security**: Rate limiting, Failed attempt lockout

#### MFA Verification Screen
- **Layout**: Centered MFA input with timer
- **Fields**: 6-digit TOTP code input
- **Actions**: Verify, Use backup code, Resend
- **Features**: Auto-focus, Auto-submit, Timer display
- **UX**: Clear error states, Recovery options

### Dashboard Screen
- **Layout**: Card-based layout with key metrics
- **Content**: User counts, Revenue metrics, System health, Recent alerts
- **Actions**: Quick actions for common tasks
- **Features**: Pull-to-refresh, Real-time updates, Search
- **Navigation**: Tab-based with bottom navigation

### User Management Screens

#### User List Screen
- **Layout**: Search bar + Filterable user list
- **Content**: User cards with key info, Status indicators
- **Actions**: Search, Filter, Bulk operations, Export
- **Features**: Infinite scroll, Advanced filters, Quick actions
- **UX**: Smooth animations, Loading states, Empty states

#### User Details Screen
- **Layout**: Tabbed interface (Profile, Subscription, Activity)
- **Content**: User info, Subscription details, Activity log
- **Actions**: Edit user, Manage subscription, Impersonate
- **Features**: Real-time updates, Action confirmations
- **UX**: Clear information hierarchy, Easy navigation

### Support Ticket Screens

#### Ticket List Screen
- **Layout**: Filterable list with status indicators
- **Content**: Ticket cards with priority, Assignee, Status
- **Actions**: Filter by status, Search, Create new ticket
- **Features**: Real-time updates, Swipe actions, Badge notifications
- **UX**: Clear priority indicators, Quick status changes

#### Ticket Detail Screen
- **Layout**: Conversation view with action bar
- **Content**: Ticket details, Message thread, Customer info
- **Actions**: Reply, Assign, Change status, Escalate
- **Features**: Rich text reply, File attachments, Canned responses
- **UX**: Smooth scrolling, Keyboard handling, Auto-save drafts

### System Monitoring Screens

#### Health Monitor Screen
- **Layout**: Grid of health status cards
- **Content**: Service status, Performance metrics, Error rates
- **Actions**: Refresh details, View logs, Restart services
- **Features**: Real-time updates, Color-coded status, Alert thresholds
- **UX**: At-a-glance status, Drill-down capabilities

#### Metrics Dashboard Screen
- **Layout**: Chart-based metrics display
- **Content**: User growth, Revenue trends, System performance
- **Actions**: Filter by date range, Export data, Detailed view
- **Features**: Interactive charts, Real-time data, Custom timeframes
- **UX**: Smooth chart animations, Touch interactions

## State Management

### Zustand Stores
```typescript
// Admin authentication store
interface AdminAuthStore {
  admin: Admin | null;
  token: string | null;
  isAuthenticated: boolean;
  mfaVerified: boolean;
  biometricEnabled: boolean;
  sessionTimeout: number;
  
  // Actions
  login: (credentials: AdminCredentials) => Promise<void>;
  verifyMFA: (code: string) => Promise<void>;
  logout: () => void;
  refreshToken: () => Promise<void>;
  setupBiometric: () => Promise<void>;
}

// Admin operations store
interface AdminOpsStore {
  users: User[];
  tickets: SupportTicket[];
  systemHealth: SystemHealth;
  metrics: SystemMetrics;
  alerts: Alert[];
  
  // Actions
  loadUsers: (filters?: UserFilters) => Promise<void>;
  updateUser: (userId: string, data: UpdateUserData) => Promise<void>;
  loadTickets: (filters?: TicketFilters) => Promise<void>;
  updateTicket: (ticketId: string, data: UpdateTicketData) => Promise<void>;
  loadSystemHealth: () => Promise<void>;
  loadMetrics: (timeframe: Timeframe) => Promise<void>;
}

// Real-time store
interface RealTimeStore {
  isConnected: boolean;
  notifications: Notification[];
  liveMetrics: LiveMetrics;
  
  // Actions
  connect: () => void;
  disconnect: () => void;
  addNotification: (notification: Notification) => void;
  updateMetrics: (metrics: Partial<LiveMetrics>) => void;
}

// Offline store
interface OfflineStore {
  isOnline: boolean;
  pendingActions: PendingAction[];
  syncQueue: SyncItem[];
  
  // Actions
  addToSyncQueue: (item: SyncItem) => void;
  processSyncQueue: () => Promise<void>;
  clearSyncQueue: () => void;
}
```

### React Query Configuration
```typescript
// Admin API queries
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 2 * 60 * 1000, // 2 minutes for admin data
      cacheTime: 5 * 60 * 1000, // 5 minutes cache
      retry: 2,
      refetchOnWindowFocus: false,
      refetchInterval: 30000, // Refresh every 30 seconds
    },
    mutations: {
      retry: 1,
    },
  },
});

// Custom hooks
export const useAdminUsersQuery = (filters?: UserFilters) => {
  return useQuery({
    queryKey: ['admin-users', filters],
    queryFn: () => adminApi.getUsers(filters),
    staleTime: 1 * 60 * 1000, // 1 minute for user data
  });
};

export const useAdminTicketsQuery = (filters?: TicketFilters) => {
  return useQuery({
    queryKey: ['admin-tickets', filters],
    queryFn: () => adminApi.getTickets(filters),
    staleTime: 30 * 1000, // 30 seconds for tickets
  });
};
```

## Real-time Features

### WebSocket Integration
```typescript
// Socket.IO client for real-time admin updates
const socket = io(ADMIN_API_URL, {
  auth: {
    token: getAdminToken(),
  },
});

// Real-time events
socket.on('user:registered', (user) => {
  // Update user count, show notification
});

socket.on('ticket:created', (ticket) => {
  // Update ticket list, show alert
});

socket.on('system:alert', (alert) => {
  // Show system alert, update health status
});

socket.on('metrics:update', (metrics) => {
  // Update dashboard metrics in real-time
});

socket.on('emergency:maintenance', (info) => {
  // Show maintenance mode notification
});
```

### Push Notifications
```typescript
// Push notification setup
const setupPushNotifications = async () => {
  const permission = await Notifications.requestPermissionsAsync();
  if (permission.status !== 'granted') return;
  
  const token = await Notifications.getExpoPushTokenAsync();
  await adminApi.registerPushToken(token.data);
  
  // Handle notifications
  Notifications.addNotificationReceivedListener(handleNotification);
  Notifications.addNotificationResponseReceivedListener(handleNotificationResponse);
};

const handleNotification = (notification) => {
  // Handle incoming notification
  const { data } = notification.request.content;
  
  switch (data.type) {
    case 'urgent_ticket':
      showUrgentTicketAlert(data.ticketId);
      break;
    case 'system_alert':
      showSystemAlert(data.alertId);
      break;
    case 'security_breach':
      showSecurityAlert(data.details);
      break;
  }
};
```

## Security Features

### Enhanced Authentication
```typescript
// Multi-factor authentication
const AdminAuthScreen = () => {
  const [mfaRequired, setMfaRequired] = useState(false);
  const [biometricAvailable, setBiometricAvailable] = useState(false);
  
  useEffect(() => {
    checkBiometricAvailability();
  }, []);
  
  const handleLogin = async (credentials) => {
    try {
      const response = await adminApi.login(credentials);
      
      if (response.mfaRequired) {
        setMfaRequired(true);
      } else {
        await completeAuthentication(response.token);
      }
    } catch (error) {
      // Handle login error
    }
  };
  
  const handleBiometricAuth = async () => {
    const result = await LocalAuthentication.authenticateAsync({
      promptText: 'Authenticate to access admin panel',
      disableDeviceFallback: false,
    });
    
    if (result.success) {
      // Proceed with biometric authentication
    }
  };
};

// Session management
const useSessionManager = () => {
  const sessionTimeout = useRef<NodeJS.Timeout>();
  
  const resetSessionTimeout = () => {
    clearTimeout(sessionTimeout.current);
    sessionTimeout.current = setTimeout(() => {
      showSessionTimeoutWarning();
    }, 8 * 60 * 60 * 1000); // 8 hours
  };
  
  const extendSession = async () => {
    await adminApi.refreshToken();
    resetSessionTimeout();
  };
  
  return { resetSessionTimeout, extendSession };
};
```

### Data Protection
- **Encryption**: All local data encrypted
- **Certificate Pinning**: SSL certificate validation
- **Jailbreak Detection**: Root detection for security
- **Screen Recording Prevention**: Block screen capture in sensitive areas
- **Session Timeout**: Automatic logout after inactivity

## Offline Support Architecture

### Offline Capabilities
```typescript
// Offline data management
const OfflineDataManager = {
  // Cache critical admin data
  cacheUsers: async (users: User[]) => {
    await AsyncStorage.setItem('cached_users', JSON.stringify(users));
  },
  
  getCachedUsers: async (): Promise<User[]> => {
    const cached = await AsyncStorage.getItem('cached_users');
    return cached ? JSON.parse(cached) : [];
  },
  
  // Queue admin actions for sync
  queueAction: async (action: AdminAction) => {
    const queue = await getActionQueue();
    queue.push({
      ...action,
      id: generateId(),
      timestamp: Date.now(),
      synced: false,
    });
    await AsyncStorage.setItem('action_queue', JSON.stringify(queue));
  },
  
  // Sync queued actions
  syncActions: async () => {
    const queue = await getActionQueue();
    const unsynced = queue.filter(action => !action.synced);
    
    for (const action of unsynced) {
      try {
        await adminApi.executeAction(action);
        action.synced = true;
      } catch (error) {
        // Handle sync error
      }
    }
    
    await AsyncStorage.setItem('action_queue', JSON.stringify(queue));
  },
};
```

### Offline Features
- **View cached user data** and basic information
- **Create support ticket drafts** for later sync
- **View system health status** (last known)
- **Read cached notifications** and alerts
- **Queue admin actions** for sync when online

## Performance Optimizations

### Mobile Performance
- **FlatList optimization**: Efficient list rendering
- **Image caching**: FastImage with memory management
- **Component memoization**: React.memo for expensive components
- **Lazy loading**: Code splitting for screens
- **Background processing**: Efficient sync operations

### Network Optimizations
- **Request batching**: Group multiple API calls
- **Request deduplication**: Prevent duplicate requests
- **Compression**: Gzip compression for API responses
- **Connection pooling**: Efficient network resource usage

### Memory Management
```typescript
// Memory optimization for large datasets
const UserList = ({ users }) => {
  const renderUser = useCallback(({ item: user }) => (
    <UserCard user={user} />
  ), []);
  
  const getItemLayout = useCallback((data, index) => ({
    length: ITEM_HEIGHT,
    offset: ITEM_HEIGHT * index,
    index,
  }), []);
  
  return (
    <FlatList
      data={users}
      renderItem={renderUser}
      getItemLayout={getItemLayout}
      removeClippedSubviews={true}
      maxToRenderPerBatch={10}
      windowSize={10}
      initialNumToRender={15}
    />
  );
};
```

## Testing Strategy

### Unit Testing
```bash
# Run unit tests
npm test

# Run with coverage
npm run test:coverage

# Watch mode
npm run test:watch
```

### Integration Testing
```bash
# Run integration tests
npm run test:integration

# API integration tests
npm run test:api

# Authentication flow tests
npm run test:auth
```

### E2E Testing
```bash
# Run E2E tests
npm run test:e2e

# Device testing
npm run test:device

# Security testing
npm run test:security
```

## Development Setup

### Prerequisites
- Node.js 18+
- React Native CLI
- Android Studio (Android development)
- Xcode (iOS development)
- Expo CLI (optional)

### Environment Variables

Create a `.env` file:

```bash
# API Configuration
ADMIN_API_URL=https://admin-api.resumebuilder.com
SSO_URL=https://sso.resumebuilder.com

# App Configuration
APP_NAME=Resume Builder Admin
APP_VERSION=1.0.0
ENVIRONMENT=development

# Security
BIOMETRIC_AUTH_ENABLED=true
SESSION_TIMEOUT=28800000
CERTIFICATE_PINNING=true

# Push Notifications
PUSH_NOTIFICATION_KEY=<your-push-key>
NOTIFICATION_SOUND=default

# Features
ENABLE_OFFLINE_MODE=true
ENABLE_REAL_TIME_ALERTS=true
ENABLE_BULK_OPERATIONS=true

# Analytics
ANALYTICS_API_KEY=<your-analytics-key>
CRASHLYTICS_API_KEY=<your-crashlytics-key>
```

### Quick Start

```bash
# Install dependencies
npm install

# iOS setup
cd ios && pod install && cd ..

# Start Metro bundler
npm start

# Run on iOS
npm run ios

# Run on Android
npm run android

# Run tests
npm test

# Build for production
npm run build
```

## Performance Targets

- **App Launch Time**: < 3s (cold start), < 1s (warm start)
- **Authentication Flow**: < 2s (including MFA)
- **User Search**: < 500ms
- **Dashboard Load**: < 2s
- **Real-time Updates**: < 100ms latency
- **Offline Sync**: < 10s for typical actions
- **Memory Usage**: < 200MB (average), < 400MB (peak)

## Accessibility

### Mobile Accessibility
- **Screen Reader**: VoiceOver (iOS) and TalkBack (Android) support
- **Dynamic Type**: Adjustable text sizes
- **High Contrast**: High contrast mode support
- **Switch Control**: Alternative input methods
- **Reduced Motion**: Respect motion preferences

## Platform-Specific Features

### iOS Features
- **Face ID / Touch ID**: Biometric authentication
- **Widget Support**: Quick admin actions from home screen
- **Apple Watch**: Admin alerts and quick actions
- **Background App Refresh**: Efficient background sync
- **Spotlight Search**: Search users and tickets

### Android Features
- **Fingerprint Authentication**: Biometric security
- **App Shortcuts**: Quick access to common actions
- **Widget Support**: Home screen widgets
- **Notification Channels**: Granular notification control
- **Material Design 3**: Modern Android design

## Development Plan

See [HIGH_LEVEL_PLAN.md](./HIGH_LEVEL_PLAN.md) for complete dependency-tree based development plan with MVPs, epics, and critical path.

## GitHub Issues

Track development progress in [GitHub Issues](https://github.com/MEYWD-Labs/Resume-Mobile-Admin/issues):
- [MVP-1 Epics](https://github.com/MEYWD-Labs/Resume-Mobile-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-1) - Core Admin Mobile Experience
- [MVP-2 Epics](https://github.com/MEYWD-Labs/Resume-Mobile-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-2) - User & Subscription Management
- [MVP-3 Epics](https://github.com/MEYWD-Labs/Resume-Mobile-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-3) - Support & System Monitoring
- [MVP-4 Epics](https://github.com/MEYWD-Labs/Resume-Mobile-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-4) - Advanced Admin Features

## Deployment

### App Store Deployment
```bash
# Build for iOS
npm run build:ios

# Build for Android
npm run build:android

# Upload to App Store
npm run deploy:ios

# Upload to Google Play
npm run deploy:android
```

### CI/CD Pipeline
- **GitHub Actions**: Automated builds and testing
- **Fastlane**: Automated deployment
- **CodePush**: OTA updates for React Native
- **Crashlytics**: Crash reporting and analytics

## Support

For issues or questions, create a GitHub issue in this repository.

## License

Proprietary - All rights reserved