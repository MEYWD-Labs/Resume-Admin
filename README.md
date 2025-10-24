# Resume-Admin

**Administrative Dashboard Frontend** for the Resume Builder SaaS platform.

## Overview

Resume-Admin is the administrative dashboard built with Next.js 14+ (App Router) and React 19. It provides comprehensive tools for managing users, subscriptions, support tickets, content, and system operations.

## Technology Stack

- **Framework**: Next.js 14+ (App Router)
- **React**: React 19 with Server Components
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: shadcn/ui
- **Tables**: TanStack Table
- **Charts**: Recharts or Chart.js
- **Forms**: React Hook Form + Zod validation
- **State Management**: Zustand + TanStack Query
- **Real-time**: WebSockets (system monitoring)
- **Deployment**: Cloudflare Pages

## Key Features

### MVP-1: Admin Foundation
- Admin authentication with MFA enforcement
- Dashboard overview (metrics, KPIs)
- User management interface (search, filter, edit)
- Activity log viewer
- Admin role management

### MVP-2: Financial & Support UI
- Subscription management (upgrade, downgrade, refund)
- Payment history viewer
- Revenue analytics dashboard
- Support ticket system interface
- Email notification center

### MVP-3: Content & System Management UI
- Template management interface
- Feature flag management with gradual rollout
- System health dashboard
- Database management tools
- Content moderation interface

### MVP-4: Advanced Analytics & Automation UI
- Custom report builder
- Cohort analysis visualization
- Churn prediction dashboard
- Automation workflow builder
- Integration management

## Service Dependencies

**Depends On**:
- **Resume-SSO** (MVP-4.3: MFA) - For admin authentication with MFA requirement
- **Resume-Admin-API** - For all admin operations

**No services depend on Resume-Admin** - This is an administrative application.

## Application Routes

### Authentication (MFA Required)
- `/login` - Admin sign in
- `/mfa` - MFA verification
- `/setup-mfa` - MFA setup (first-time)

### Dashboard Routes (Protected)
- `/` - Dashboard overview
- `/users` - User management
- `/users/:id` - User details
- `/users/:id/activity` - User activity log
- `/subscriptions` - Subscription management
- `/subscriptions/:id` - Subscription details
- `/payments` - Payment history
- `/analytics` - Analytics dashboard
- `/reports` - Custom reports
- `/reports/builder` - Report builder

### Support Routes
- `/tickets` - Support tickets
- `/tickets/:id` - Ticket details
- `/tickets/new` - Create ticket (on behalf of user)

### Content Management Routes
- `/templates` - Template management
- `/templates/:id/edit` - Template editor
- `/templates/new` - Create template

### System Routes
- `/features` - Feature flag management
- `/system/health` - System health dashboard
- `/system/logs` - Application logs
- `/database` - Database management
- `/workflows` - Automation workflows
- `/workflows/:id/edit` - Workflow editor

### Admin Settings
- `/settings` - Admin settings
- `/settings/admins` - Admin user management
- `/settings/security` - Security settings
- `/settings/integrations` - Integration config

## Development Setup

### Prerequisites
- Node.js 18+
- npm or pnpm
- Access to Resume-SSO and Resume-Admin-API services
- Admin credentials with MFA setup

### Environment Variables

Create a `.env.local` file:

```bash
# API Configuration
NEXT_PUBLIC_SSO_URL=http://localhost:8787
NEXT_PUBLIC_ADMIN_API_URL=http://localhost:8789

# Feature Flags
NEXT_PUBLIC_ENABLE_AUTOMATION=true
NEXT_PUBLIC_ENABLE_CUSTOM_REPORTS=true

# Analytics
NEXT_PUBLIC_ANALYTICS_ID=<cloudflare-analytics-id>
```

### Quick Start

```bash
# Install dependencies
npm install

# Run development server
npm run dev

# Build for production
npm run build

# Start production server
npm start

# Run tests
npm test

# Run E2E tests
npm run test:e2e
```

## Project Structure

```
src/
├── app/                  # Next.js 14 App Router
│   ├── (auth)/          # Auth routes (login, MFA)
│   ├── (dashboard)/     # Protected dashboard routes
│   │   ├── users/      # User management
│   │   ├── subscriptions/ # Subscription management
│   │   ├── tickets/    # Support tickets
│   │   ├── templates/  # Template management
│   │   ├── features/   # Feature flags
│   │   ├── system/     # System management
│   │   └── workflows/  # Automation workflows
│   └── api/            # API routes (proxy to backend)
├── components/         # React components
│   ├── ui/            # shadcn/ui components
│   ├── dashboard/     # Dashboard components
│   ├── tables/        # Data table components
│   ├── charts/        # Chart components
│   └── shared/        # Shared components
├── lib/               # Utility functions
│   ├── api.ts         # API client
│   ├── auth.ts        # Auth helpers
│   └── utils.ts       # General utilities
├── hooks/             # Custom React hooks
├── stores/            # Zustand stores
└── types/             # TypeScript types
```

## Admin Roles & UI Access

### Super Admin
- Full access to all features
- User management (all operations)
- Financial operations (refunds, adjustments)
- System configuration
- Database management
- Admin user management

### Admin
- User management (view, suspend)
- Content management
- All support tickets
- Analytics (read-only)
- Template management
- Limited system access

### Support
- View users (limited fields)
- Manage assigned support tickets
- Basic user actions (password reset)
- No financial access
- No system access

## Key Components

### Dashboard Overview
- Real-time metrics (users, revenue, system health)
- Quick actions (recent tickets, pending approvals)
- Activity feed
- System alerts

### User Management Table
- Searchable and filterable
- Bulk actions (suspend, delete)
- Export to CSV
- Column customization
- Pagination with server-side sorting

### Analytics Dashboard
- User growth charts
- Revenue trends
- Subscription distribution
- Churn rate visualization
- Cohort analysis

### Support Ticket System
- Ticket inbox with filters
- Conversation thread view
- Status and priority management
- Assignment to admins
- Email integration

### Feature Flag Manager
- Toggle features on/off
- Gradual rollout with percentage
- Target specific tiers
- Preview impact before enabling
- A/B testing configuration

### System Health Dashboard
- Real-time monitoring
- Database metrics
- API response times
- Error rate tracking
- Worker statistics

## State Management

### Zustand Stores
- `useAuthStore` - Admin authentication state
- `useUserStore` - User management state
- `useTicketStore` - Support ticket state
- `useSystemStore` - System health monitoring

### TanStack Query
- Server state caching
- Automatic background refetching
- Optimistic updates
- Pagination and infinite scroll

## Authentication Flow

1. Admin logs in via Resume-SSO
2. MFA verification required (TOTP)
3. JWT tokens stored in httpOnly cookies
4. Middleware validates admin role + MFA on all routes
5. Session expires after 8 hours of inactivity

## Performance Optimizations

- Server Components for static admin content
- Streaming SSR for dashboard metrics
- Image optimization with Next.js Image
- Table virtualization for large datasets
- Chart data lazy loading
- Bundle optimization

## Performance Targets

- Dashboard load time: **< 2s**
- User search: **< 500ms**
- Chart rendering: **< 1s**
- Table pagination: **< 300ms**
- Report generation: **< 10s**

## Accessibility

- WCAG 2.1 Level AA compliance
- Keyboard navigation support
- Screen reader friendly tables
- High contrast mode
- Focus indicators
- ARIA labels and roles

## Security Features

- MFA enforcement for all admin access
- Session timeout after inactivity
- IP whitelisting (optional)
- Audit log for all admin actions
- Role-based UI permissions
- CSRF protection

## Testing

```bash
# Run unit tests
npm test

# Run E2E tests (Playwright)
npm run test:e2e

# Run accessibility tests
npm run test:a11y

# Test admin permissions
npm run test:permissions

# Test coverage
npm run test:coverage
```

## Development Plan

See [HIGH_LEVEL_PLAN.md](./HIGH_LEVEL_PLAN.md) for the complete dependency-tree based development plan with MVPs, epics, and critical path.

## GitHub Issues

Track development progress in [GitHub Issues](https://github.com/MEYWD-Labs/Resume-Admin/issues):
- [MVP-1 Epics](https://github.com/MEYWD-Labs/Resume-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-1) - Admin Foundation
- [MVP-2 Epics](https://github.com/MEYWD-Labs/Resume-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-2) - Financial & Support UI
- [MVP-3 Epics](https://github.com/MEYWD-Labs/Resume-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-3) - Content & System Management UI
- [MVP-4 Epics](https://github.com/MEYWD-Labs/Resume-Admin/issues?q=is%3Aissue+is%3Aopen+MVP-4) - Advanced Analytics & Automation UI

## Deployment

```bash
# Build and deploy to Cloudflare Pages
npm run deploy

# Preview deployment
npm run deploy:preview

# Production deployment
npm run deploy:production
```

## Monitoring & Alerts

- Real-time dashboard for system monitoring
- Admin activity logging
- Error tracking and alerting
- Performance metrics
- Security alerts (failed MFA attempts, suspicious activity)

## Browser Support

- Chrome/Edge: Latest 2 versions
- Firefox: Latest 2 versions
- Safari: Latest 2 versions
- No mobile browser support (desktop-only admin dashboard)

## Support

For issues or questions, create a GitHub issue in this repository.

## License

Proprietary - All rights reserved
