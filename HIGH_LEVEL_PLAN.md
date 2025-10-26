# High-Level Development Plan - Resume-Admin

**Service Type**: Admin Dashboard Frontend (Next.js 14+ on Cloudflare Pages)
**Build Approach**: AI-Automated Development
**Priority**: MEDIUM - Administrative interface

---

## Dependency Tree Overview

Resume-Admin (Admin Dashboard - Web)
├── DEPENDS ON: Resume-SSO (MVP-1.4: Login, MVP-4.3: MFA) → For admin authentication
├── DEPENDS ON: Resume-Admin-API (All MVPs) → For all backend operations
├── BLOCKS: None (this is an end-user application)

External Dependencies:
├── Resume-SSO (MVP-1.4: Login System) → For basic authentication
├── Resume-SSO (MVP-4.3: MFA) → For admin-required MFA
├── Resume-Admin-API (MVP-1: Admin Foundation) → For admin management, analytics
├── Resume-Admin-API (MVP-2: Financial) → For billing, payments, support
├── Resume-Admin-API (MVP-3: Content & System) → For content, websites, flags
└── Resume-Admin-API (MVP-4: Advanced) → For reports, A/B testing, automation

---

## MVP-1: Admin Foundation (Foundation)

**Status**: MUST BUILD AFTER Resume-SSO and Resume-Admin-API are ready
**Dependencies**: Resume-SSO (Admin Auth + MFA), Resume-Admin-API (MVP-1)
**Blocks**: All admin operations

### Epic 1.1: Admin Authentication with MFA
**What**: Admin login and authentication flow
**Complexity**: Medium
**Dependencies**: Resume-SSO (MVP-1.4: Login, MVP-4.3: MFA)

**Tasks**:
- [ ] `/admin/login` page with email/password form
- [ ] Integration with Resume-SSO for admin authentication
- [ ] MFA verification page (TOTP required for all admins)
- [ ] Session management with JWT
- [ ] Protected route middleware (admin-only routes)
- [ ] Role-based access control (Super Admin, Admin, Support)
- [ ] Logout functionality
- [ ] Session timeout (30 minutes of inactivity)
- [ ] Admin context provider (current admin user, role, permissions)

**Acceptance Criteria**:
- [ ] Only users with admin roles can access dashboard
- [ ] MFA required for all admin logins
- [ ] Role-based UI rendering (show/hide features based on role)
- [ ] Sessions expire after 30 minutes of inactivity
- [ ] Secure JWT storage (httpOnly cookies)

---

### Epic 1.2: Dashboard Overview
**What**: Main admin dashboard with key metrics
**Complexity**: Large
**Dependencies**: Epic 1.1 (Admin Auth), Resume-Admin-API (MVP-1.2: Analytics)

**Tasks**:
- [ ] `/admin/dashboard` page layout
- [ ] Key metrics cards:
  - [ ] Total users (all time)
  - [ ] Active users (last 30 days)
  - [ ] Total resumes created
  - [ ] PDF exports (last 30 days)
  - [ ] Conversion rate (free → paid)
- [ ] Quick stats section:
  - [ ] New signups today/week/month
  - [ ] Revenue today/week/month
  - [ ] Active subscriptions
  - [ ] Open support tickets
- [ ] User growth chart (line chart, daily/weekly/monthly)
- [ ] Subscription distribution chart (pie chart: Free, Pro, Premium)
- [ ] Popular templates chart (bar chart, top 10)
- [ ] Activity timeline (recent registrations, subscriptions, cancellations)
- [ ] Date range selector
- [ ] Real-time updates (refresh every 60 seconds)
- [ ] Loading states and skeletons
- [ ] Error handling

**Acceptance Criteria**:
- [ ] Dashboard loads in < 2 seconds
- [ ] All metrics accurate
- [ ] Charts render correctly (Recharts or Chart.js)
- [ ] Date range selector works
- [ ] Responsive design (desktop, tablet)
- [ ] Real-time updates without page reload

---

### Epic 1.3: User Management UI
**What**: Interface for managing platform users
**Complexity**: Large
**Dependencies**: Epic 1.1 (Admin Auth), Resume-Admin-API (MVP-1.3: User Management)

**Tasks**:
- [ ] `/admin/users` page with table view
- [ ] Paginated user table (50 users per page, TanStack Table)
- [ ] Table columns: email, name, subscription tier, status, signup date, last login
- [ ] Search functionality (email, name, ID)
- [ ] Filters:
  - [ ] Subscription tier (Free, Pro, Premium)
  - [ ] Status (Active, Suspended, Deleted)
  - [ ] Signup date range
  - [ ] Last active date range
- [ ] Sort by any column
- [ ] Bulk action support (select multiple users)
- [ ] `/admin/users/:id` user details page:
  - [ ] User profile information
  - [ ] Account statistics
  - [ ] Subscription status card
  - [ ] Resume list (with previews)
  - [ ] Export history table
  - [ ] Activity log timeline
  - [ ] Email verification status
- [ ] User action buttons:
  - [ ] View user resumes (read-only)
  - [ ] Suspend account (with confirmation modal)
  - [ ] Unsuspend account
  - [ ] Reset password (sends email)
  - [ ] Verify email manually
  - [ ] Delete account (with confirmation modal)
  - [ ] Impersonate user (for support, opens new tab with user session)
- [ ] Loading states for all actions
- [ ] Success/error toasts

**Acceptance Criteria**:
- [ ] User table loads quickly (< 500ms)
- [ ] Search returns results in < 300ms
- [ ] Filters work correctly
- [ ] User details page comprehensive
- [ ] All user actions functional
- [ ] Impersonation works (support can view as user)
- [ ] Role-based permissions enforced (Support can't delete users)
- [ ] Responsive layout

---

### Epic 1.4: Template Management UI
**What**: Manage resume templates
**Complexity**: Medium
**Dependencies**: Epic 1.1 (Admin Auth), Resume-Admin-API (MVP-1.4: Templates)

**Tasks**:
- [ ] `/admin/templates` page with grid view
- [ ] Template cards with preview images
- [ ] Template metadata display (name, category, premium status, usage count)
- [ ] Filter by category (Modern, Classic, ATS-Friendly, etc.)
- [ ] Filter by premium status (free, premium)
- [ ] Sort by usage, date created
- [ ] `/admin/templates/:id` template details page:
  - [ ] Full template preview
  - [ ] Template metadata form
  - [ ] Usage statistics
  - [ ] HTML/CSS editor
- [ ] `/admin/templates/new` create template page:
  - [ ] Upload template files
  - [ ] HTML/CSS editor with syntax highlighting
  - [ ] Preview pane (live preview)
  - [ ] Template metadata form (name, category, premium)
  - [ ] Upload preview image (to R2)
- [ ] Template actions:
  - [ ] Edit template
  - [ ] Delete template (with confirmation)
  - [ ] Feature/unfeature template
  - [ ] Set as premium/free
- [ ] Template versioning (view previous versions)
- [ ] Loading states and error handling

**Acceptance Criteria**:
- [ ] Template grid displays all templates
- [ ] Filters and sort work
- [ ] Template preview accurate
- [ ] HTML/CSS editor functional
- [ ] Live preview works
- [ ] Template files upload to R2
- [ ] Usage statistics displayed
- [ ] Only Admins and Super Admins can edit/delete
- [ ] Responsive design

---

### Epic 1.5: Admin Management UI
**What**: Manage admin users and roles
**Complexity**: Small
**Dependencies**: Epic 1.1 (Admin Auth), Resume-Admin-API (MVP-1.1: Admin Management)

**Tasks**:
- [ ] `/admin/admins` page with table view
- [ ] List all admin users
- [ ] Table columns: name, email, role, created date, created by
- [ ] Add new admin button (modal or page)
- [ ] Create admin form:
  - [ ] Email input (must be existing user)
  - [ ] Role selection (Super Admin, Admin, Support)
- [ ] Edit admin role (inline or modal)
- [ ] Remove admin access (with confirmation)
- [ ] Activity log per admin (recent actions)
- [ ] Only Super Admins can manage other admins

**Acceptance Criteria**:
- [ ] Admin list displays correctly
- [ ] Super Admins can create/edit/delete admins
- [ ] Non-Super Admins can't access admin management
- [ ] Activity log shows recent actions per admin
- [ ] Email validation (user must exist)

---

## MVP-2: Financial & Support UI (Monetization)

**Status**: Required for revenue and support management
**Dependencies**: MVP-1, Resume-Admin-API (MVP-2)
**Blocks**: Financial operations and customer support

### Epic 2.1: Financial Dashboard
**What**: Revenue metrics and financial analytics
**Complexity**: Large
**Dependencies**: Epic 1.2 (Dashboard), Resume-Admin-API (MVP-2.2: Revenue Analytics)

**Tasks**:
- [ ] `/admin/finance` page layout
- [ ] Revenue metrics cards:
  - [ ] MRR (Monthly Recurring Revenue)
  - [ ] ARR (Annual Recurring Revenue)
  - [ ] Revenue growth rate (month-over-month)
  - [ ] ARPU (Average Revenue Per User)
  - [ ] LTV (Lifetime Value)
- [ ] Revenue charts:
  - [ ] Revenue over time (line chart)
  - [ ] Revenue by plan (stacked bar chart)
  - [ ] Churn rate visualization
  - [ ] New vs lost revenue
- [ ] Subscription overview:
  - [ ] Active subscriptions by tier
  - [ ] Upcoming renewals (next 7 days, 30 days)
  - [ ] Failed payments count
  - [ ] Cancellation requests
- [ ] Date range selector (last 7 days, 30 days, 90 days, custom)
- [ ] Export financial data to CSV
- [ ] Real-time updates (refresh every 5 minutes)

**Acceptance Criteria**:
- [ ] All financial metrics accurate
- [ ] Charts render correctly
- [ ] Date range selector works
- [ ] CSV export functional
- [ ] Only Super Admins and Admins can view
- [ ] Responsive layout

---

### Epic 2.2: Payment Management UI
**What**: Manage subscriptions, transactions, and refunds
**Complexity**: Large
**Dependencies**: Epic 2.1 (Financial Dashboard), Resume-Admin-API (MVP-2.1, 2.3)

**Tasks**:
- [ ] `/admin/subscriptions` page with table view
- [ ] Subscription table:
  - [ ] Columns: user email, plan, status, start date, next billing
  - [ ] Pagination (50 per page)
  - [ ] Filters: plan (Pro, Premium), status (active, canceled, expired)
  - [ ] Search by user email
- [ ] Subscription actions:
  - [ ] View subscription details
  - [ ] Cancel subscription (with confirmation)
  - [ ] Refund subscription (with amount input, confirmation)
- [ ] `/admin/transactions` page with table view
- [ ] Transaction table:
  - [ ] Columns: user email, amount, status, date, payment method
  - [ ] Pagination (50 per page)
  - [ ] Filters: status (succeeded, failed, refunded), date range
  - [ ] Search by user email, transaction ID
- [ ] Transaction actions:
  - [ ] View transaction details
  - [ ] Refund transaction (full or partial)
- [ ] Failed payments section:
  - [ ] List failed payments
  - [ ] Retry payment button
  - [ ] Contact user button (opens support ticket)
- [ ] Loading states and error handling
- [ ] Success/error toasts

**Acceptance Criteria**:
- [ ] Subscription and transaction tables load quickly
- [ ] Filters and search work
- [ ] Cancel subscription functional
- [ ] Refunds processed correctly (via API → Stripe)
- [ ] Failed payments highlighted
- [ ] Only Super Admins can refund
- [ ] All actions logged

---

### Epic 2.3: Coupon Management UI
**What**: Create and manage discount coupons
**Complexity**: Medium
**Dependencies**: Epic 2.1 (Financial Dashboard), Resume-Admin-API (MVP-2.4: Coupons)

**Tasks**:
- [ ] `/admin/coupons` page with table view
- [ ] Coupon table:
  - [ ] Columns: code, type, value, uses/max, expires, status
  - [ ] Pagination
  - [ ] Filters: status (active, expired), type (percentage, fixed)
- [ ] `/admin/coupons/new` create coupon page:
  - [ ] Coupon code input (auto-generate option)
  - [ ] Type selector (percentage, fixed amount)
  - [ ] Value input
  - [ ] Max uses input (optional)
  - [ ] Expiration date picker
  - [ ] Applicable plans (Pro, Premium, both)
- [ ] `/admin/coupons/:id` coupon details page:
  - [ ] Coupon metadata
  - [ ] Usage statistics (redemptions over time)
  - [ ] Revenue impact calculation
  - [ ] List of users who redeemed
- [ ] Coupon actions:
  - [ ] Edit coupon
  - [ ] Delete coupon (with confirmation)
  - [ ] Deactivate coupon
- [ ] Coupon analytics chart (redemptions over time)

**Acceptance Criteria**:
- [ ] Coupon creation functional
- [ ] All coupon fields validated
- [ ] Auto-generate coupon code works
- [ ] Usage statistics accurate
- [ ] Revenue impact calculated
- [ ] Only Admins and Super Admins can create
- [ ] Expired coupons clearly marked

---

### Epic 2.4: Support Ticket System UI
**What**: Manage customer support tickets
**Complexity**: Large
**Dependencies**: Epic 1.3 (User Management), Resume-Admin-API (MVP-2.5: Support)

**Tasks**:
- [ ] `/admin/tickets` page with table view
- [ ] Ticket table:
  - [ ] Columns: ticket ID, user email, subject, status, priority, assigned to, created date
  - [ ] Pagination (50 per page)
  - [ ] Filters: status (open, in_progress, resolved, closed), priority (low, medium, high, urgent)
  - [ ] Search by ticket ID, user email, subject
  - [ ] Sort by created date, updated date, priority
- [ ] Ticket status badges (color-coded)
- [ ] Priority indicators (icons or colors)
- [ ] Assigned admin display
- [ ] `/admin/tickets/:id` ticket details page:
  - [ ] Ticket metadata (status, priority, assigned to)
  - [ ] Conversation history (chronological)
  - [ ] User information sidebar (quick access to user profile)
  - [ ] Reply form (rich text editor)
  - [ ] Canned responses dropdown
  - [ ] File attachment support
  - [ ] Internal notes section (not visible to user)
  - [ ] Quick actions: view user profile, suspend user, refund
- [ ] Ticket actions:
  - [ ] Reply to ticket (sends email to user)
  - [ ] Change status (open, in_progress, resolved, closed)
  - [ ] Change priority
  - [ ] Assign to admin (dropdown)
  - [ ] Add internal note
  - [ ] Close ticket (with resolution summary)
- [ ] Create ticket button (manual ticket creation)
- [ ] Real-time updates (new tickets, replies)
- [ ] Notification badge for new/unread tickets

**Acceptance Criteria**:
- [ ] Ticket table loads quickly
- [ ] Filters and search work
- [ ] Ticket details show full conversation
- [ ] Replies sent via email to user
- [ ] Canned responses selectable
- [ ] Internal notes private
- [ ] Quick actions functional
- [ ] Support role has full access
- [ ] Real-time notifications work
- [ ] Responsive layout

---

## MVP-3: Content & System Management UI (Operations)

**Status**: Required for system operations
**Dependencies**: MVP-2, Resume-Admin-API (MVP-3)
**Blocks**: Content and system management

### Epic 3.1: Content Management UI
**What**: Manage announcements, campaigns, and content library
**Complexity**: Large
**Dependencies**: Epic 1.1 (Admin Auth), Resume-Admin-API (MVP-3.1: Content)

**Tasks**:
- [ ] `/admin/content/announcements` page:
  - [ ] Announcement table (title, status, scheduled date, target)
  - [ ] Create announcement button (opens modal or page)
  - [ ] Announcement form:
    - [ ] Title input
    - [ ] Content (rich text editor)
    - [ ] Target audience (all users, free, pro, premium)
    - [ ] Schedule date picker
    - [ ] Preview button
  - [ ] Publish/unpublish announcement
  - [ ] View count tracking
- [ ] `/admin/content/campaigns` page:
  - [ ] Email campaign table (name, status, sent date, recipients, open rate)
  - [ ] Create campaign button
  - [ ] Campaign form:
    - [ ] Campaign name
    - [ ] Email template selection
    - [ ] Subject line input
    - [ ] Content (rich text editor)
    - [ ] User segmentation (tier, activity, signup date)
    - [ ] A/B test configuration (optional)
    - [ ] Schedule send date
  - [ ] Send campaign button (with confirmation)
  - [ ] Campaign analytics (opens, clicks, unsubscribes)
- [ ] `/admin/content/library` page:
  - [ ] Example content table (category, content, usage count)
  - [ ] Add example button
  - [ ] Edit/delete examples
  - [ ] Skill taxonomies management
  - [ ] Job titles database
  - [ ] Bulk import/export (CSV)

**Acceptance Criteria**:
- [ ] Announcements created and scheduled
- [ ] Email campaigns sent to segmented users
- [ ] Campaign analytics accurate (open/click rates)
- [ ] Content library managed
- [ ] Bulk import/export works
- [ ] Only Admins and Super Admins can create

---

### Epic 3.2: Website Management UI
**What**: Manage user personal websites
**Complexity**: Medium
**Dependencies**: Epic 1.3 (User Management), Resume-Admin-API (MVP-3.2: Websites)

**Tasks**:
- [ ] `/admin/websites` page with table view
- [ ] Website table:
  - [ ] Columns: user email, subdomain, custom domain, status, created date, visits
  - [ ] Pagination
  - [ ] Filters: status (active, suspended), custom domain (yes/no)
  - [ ] Search by user email, domain
- [ ] Website stats cards:
  - [ ] Total websites created
  - [ ] Active websites
  - [ ] Custom domains in use
  - [ ] Total visits (aggregated)
- [ ] Popular website templates chart
- [ ] `/admin/websites/:id` website details page:
  - [ ] Website metadata
  - [ ] User information
  - [ ] Website preview (iframe or link)
  - [ ] Traffic statistics (if available)
  - [ ] Custom domain status
- [ ] Website actions:
  - [ ] View website (opens in new tab)
  - [ ] Suspend website (abuse/policy violation)
  - [ ] Delete website (with confirmation)
- [ ] `/admin/domains` page:
  - [ ] Custom domain table (domain, user, status, verified date)
  - [ ] Verify domain button
  - [ ] Approve/reject domain button
  - [ ] DNS configuration display

**Acceptance Criteria**:
- [ ] Website table displays all user sites
- [ ] Suspend website functional
- [ ] Custom domains verified manually
- [ ] DNS management functional
- [ ] Website preview works
- [ ] Only Admins and Super Admins can manage

---

### Epic 3.3: Feature Flag Management UI
**What**: Control feature rollouts and A/B tests
**Complexity**: Large
**Dependencies**: Epic 1.1 (Admin Auth), Resume-Admin-API (MVP-3.3: Feature Flags)

**Tasks**:
- [ ] `/admin/flags` page with table view
- [ ] Feature flag table:
  - [ ] Columns: flag name, status (enabled/disabled), rollout %, target, updated date
  - [ ] Toggle switches for quick enable/disable
  - [ ] Filters: status (enabled, disabled)
- [ ] `/admin/flags/new` create flag page:
  - [ ] Flag name input
  - [ ] Description textarea
  - [ ] Enable/disable toggle
  - [ ] Gradual rollout slider (0-100%)
  - [ ] User segment targeting:
    - [ ] By subscription tier
    - [ ] By location
    - [ ] By user list (CSV upload)
  - [ ] A/B test configuration (optional):
    - [ ] Variant A/B names
    - [ ] Traffic split percentage
- [ ] `/admin/flags/:id` flag details page:
  - [ ] Flag metadata
  - [ ] Current rollout percentage
  - [ ] Targeting rules display
  - [ ] Usage statistics (how many users see this flag)
  - [ ] A/B test results (if applicable)
  - [ ] Change history (who changed what when)
- [ ] Flag actions:
  - [ ] Edit flag
  - [ ] Toggle enable/disable
  - [ ] Adjust rollout percentage
  - [ ] Delete flag (with confirmation)
- [ ] Feature adoption chart (over time)

**Acceptance Criteria**:
- [ ] Feature flags created and toggled
- [ ] Gradual rollout works (percentage slider)
- [ ] User targeting rules enforced
- [ ] A/B tests configured
- [ ] Usage statistics accurate
- [ ] Change history logged
- [ ] Only Super Admins can manage

---

### Epic 3.4: System Health Dashboard
**What**: Monitor system health and performance
**Complexity**: Large
**Dependencies**: Epic 1.2 (Dashboard), Resume-Admin-API (MVP-3.4: System Health)

**Tasks**:
- [ ] `/admin/system` page layout
- [ ] System status cards:
  - [ ] API health (green/yellow/red status indicator)
  - [ ] Database status (query time, connection count)
  - [ ] Queue status (pending, processing, failed jobs)
  - [ ] R2 storage usage (used/total, percentage)
- [ ] Performance metrics section:
  - [ ] API response times chart (p50, p95, p99)
  - [ ] CPU usage graph (Cloudflare Workers)
  - [ ] Database query performance
  - [ ] Slow query log table
- [ ] Error tracking section:
  - [ ] Recent errors table (last 24 hours)
  - [ ] Error frequency chart
  - [ ] Error details modal (stack trace, context)
  - [ ] Link to Sentry (if integrated)
- [ ] Real-time updates (refresh every 30 seconds)
- [ ] Alert indicators (red badge if issues detected)
- [ ] Export system report (PDF)

**Acceptance Criteria**:
- [ ] System status cards accurate
- [ ] Performance charts render correctly
- [ ] Error tracking displays recent errors
- [ ] Real-time updates work
- [ ] Alerts triggered for critical issues
- [ ] Only Super Admins and Admins can view
- [ ] Responsive layout

---

## MVP-4: Advanced Analytics & Automation UI (Advanced Features)

**Status**: Advanced admin capabilities
**Dependencies**: MVP-3, Resume-Admin-API (MVP-4)
**Blocks**: Advanced reporting and automation

### Epic 4.1: Custom Report Builder UI
**What**: Build and schedule custom analytics reports
**Complexity**: Large
**Dependencies**: Epic 1.2 (Dashboard), Resume-Admin-API (MVP-4.1: Reports)

**Tasks**:
- [ ] `/admin/reports` page:
  - [ ] Saved reports table (name, created by, last run, schedule)
  - [ ] Create report button
- [ ] `/admin/reports/builder` report builder page:
  - [ ] Metric selection (checkboxes for available metrics)
  - [ ] Date range selector (custom, last 7/30/90 days)
  - [ ] Filters section (user tier, status, etc.)
  - [ ] Grouping options (by day, week, month)
  - [ ] Preview report button (shows chart/table)
  - [ ] Save report button
  - [ ] Schedule report section:
    - [ ] Frequency (daily, weekly, monthly)
    - [ ] Email recipients (admin emails)
    - [ ] Report format (CSV, PDF)
- [ ] Report preview modal (chart and table)
- [ ] Export report buttons (CSV, PDF)
- [ ] Scheduled reports management:
  - [ ] Edit schedule
  - [ ] Pause/resume schedule
  - [ ] Delete scheduled report

**Acceptance Criteria**:
- [ ] Report builder functional
- [ ] Metric selection works
- [ ] Preview displays accurate data
- [ ] Reports saved and retrievable
- [ ] Scheduled reports sent via email
- [ ] CSV and PDF exports work
- [ ] Only Admins and Super Admins can create

---

### Epic 4.2: A/B Testing Dashboard
**What**: Analyze A/B test results
**Complexity**: Medium
**Dependencies**: Epic 3.3 (Feature Flags), Resume-Admin-API (MVP-4.2: A/B Testing)

**Tasks**:
- [ ] `/admin/experiments` page:
  - [ ] Active experiments table (name, status, start date, traffic split)
  - [ ] Completed experiments table
- [ ] `/admin/experiments/:id` experiment details page:
  - [ ] Experiment metadata (name, variants, traffic split)
  - [ ] Results section:
    - [ ] Variant performance comparison (chart)
    - [ ] Conversion rates per variant
    - [ ] Statistical significance indicator
    - [ ] Confidence intervals
    - [ ] Sample size display
    - [ ] Duration tracking
  - [ ] Winner selection button (marks variant as winner, ends test)
  - [ ] Export results button (CSV, PDF)
- [ ] Experiment status badges (running, paused, completed)
- [ ] Test duration progress bar

**Acceptance Criteria**:
- [ ] Experiment list displays all tests
- [ ] Results displayed clearly
- [ ] Statistical significance calculated
- [ ] Winner selection functional
- [ ] Export results works
- [ ] Only Super Admins can view

---

### Epic 4.3: AI Usage Analytics UI
**What**: Track AI feature usage and costs
**Complexity**: Medium
**Dependencies**: Epic 1.2 (Dashboard), Resume-Admin-API (MVP-4.3: AI Analytics)

**Tasks**:
- [ ] `/admin/ai` page layout
- [ ] AI usage metrics cards:
  - [ ] Total AI requests (all time)
  - [ ] AI requests (last 30 days)
  - [ ] Most used AI feature
  - [ ] Total credits consumed
  - [ ] Estimated costs (if applicable)
- [ ] AI usage charts:
  - [ ] AI requests over time (line chart)
  - [ ] Popular AI features (bar chart)
  - [ ] Usage by subscription tier (pie chart)
  - [ ] Credits consumed per user (table, top users)
- [ ] AI feature breakdown:
  - [ ] Content suggestions usage
  - [ ] Resume analysis usage
  - [ ] Smart rewriting usage
- [ ] Cost analysis section (if applicable)
- [ ] Date range selector
- [ ] Export AI usage report (CSV)

**Acceptance Criteria**:
- [ ] AI usage metrics accurate
- [ ] Charts render correctly
- [ ] Popular features identified
- [ ] Costs calculated (if applicable)
- [ ] Export works
- [ ] Only Super Admins and Admins can view

---

### Epic 4.4: Automation Workflows UI
**What**: Configure automated admin tasks
**Complexity**: Large
**Dependencies**: Epic 1.1 (Admin Auth), Resume-Admin-API (MVP-4.4: Automation)

**Tasks**:
- [ ] `/admin/automation` page:
  - [ ] Active automations table (name, trigger, status, last run)
  - [ ] Create automation button
- [ ] `/admin/automation/new` automation builder page:
  - [ ] Automation name input
  - [ ] Trigger selection:
    - [ ] Time-based (cron-like scheduler)
    - [ ] Event-based (user signup, payment failed, etc.)
  - [ ] Condition builder (if/else logic):
    - [ ] Field selector (user tier, status, etc.)
    - [ ] Operator selector (equals, greater than, etc.)
    - [ ] Value input
  - [ ] Action selection:
    - [ ] Send email
    - [ ] Suspend account
    - [ ] Send notification
    - [ ] Update user field
    - [ ] Create support ticket
  - [ ] Action sequence (multiple actions, drag-to-reorder)
  - [ ] Test automation button (dry run)
  - [ ] Save automation button
- [ ] `/admin/automation/:id` automation details page:
  - [ ] Automation metadata
  - [ ] Trigger and action display
  - [ ] Execution history (last 10 runs, success/failure)
  - [ ] Edit automation button
  - [ ] Pause/resume toggle
  - [ ] Delete automation (with confirmation)
- [ ] Automation execution logs

**Acceptance Criteria**:
- [ ] Automation builder functional
- [ ] Trigger and condition logic works
- [ ] Actions execute correctly
- [ ] Test automation (dry run) works
- [ ] Execution history logged
- [ ] Pause/resume works
- [ ] Only Super Admins can create

---

### Epic 4.5: GDPR & Bulk Operations UI
**What**: Handle GDPR requests and bulk admin operations
**Complexity**: Medium
**Dependencies**: Epic 1.3 (User Management), Resume-Admin-API (MVP-4.5: GDPR)

**Tasks**:
- [ ] `/admin/gdpr` page:
  - [ ] Data export requests table (user, status, requested date)
  - [ ] Data deletion requests table
  - [ ] Export user data button (opens modal with user ID input)
  - [ ] Delete user data button (opens modal with user ID input, requires confirmation)
  - [ ] GDPR request queue status
- [ ] `/admin/bulk` page:
  - [ ] Bulk operations section:
    - [ ] Bulk user actions (suspend, delete, etc.)
    - [ ] Bulk email sending
    - [ ] Bulk refunds
  - [ ] CSV upload for bulk operations
  - [ ] Operation preview (shows affected users)
  - [ ] Execute bulk operation button (with confirmation)
  - [ ] Bulk operation history (last 10 operations)
- [ ] Data export result (download ZIP file)
- [ ] Audit trail display (all GDPR operations logged)

**Acceptance Criteria**:
- [ ] GDPR export generates ZIP file with all user data
- [ ] GDPR delete removes all PII
- [ ] Bulk operations queued and processed
- [ ] CSV upload validated
- [ ] Preview shows affected users
- [ ] All operations logged to audit trail
- [ ] Only Super Admins can perform GDPR operations

---

### Epic 4.6: Admin Activity Log UI
**What**: View and search admin action logs
**Complexity**: Medium
**Dependencies**: Epic 1.5 (Admin Management), Resume-Admin-API (MVP-1.5: Audit Log)

**Tasks**:
- [ ] `/admin/audit` page:
  - [ ] Audit log table:
    - [ ] Columns: admin email, action, resource, timestamp, IP address, changes
    - [ ] Pagination (50 per page)
    - [ ] Filters: admin, action type, resource type, date range
    - [ ] Search by admin email, resource ID
  - [ ] Action details modal (shows full context, before/after changes)
  - [ ] Export audit log button (CSV)
- [ ] Date range selector (last 7/30/90 days, custom)
- [ ] Real-time updates (new actions appear without refresh)

**Acceptance Criteria**:
- [ ] Audit log displays all admin actions
- [ ] Filters and search work
- [ ] Action details comprehensive
- [ ] Changes displayed clearly (before/after)
- [ ] CSV export functional
- [ ] Only Super Admins can view full audit log
- [ ] Other admins see only their own actions

---

## Critical Path (Build Order)

1. **MVP-1.1**: Admin Authentication ⟶ Foundation (blocks all admin features)
2. **MVP-1.2**: Dashboard Overview ⟶ Main landing page
3. **MVP-1.3**: User Management UI ⟶ Core admin feature
4. **MVP-1.4**: Template Management UI ⟶ Content control
5. **MVP-1.5**: Admin Management UI ⟶ Admin control
6. **MVP-2.1**: Financial Dashboard ⟶ Revenue visibility
7. **MVP-2.2**: Payment Management UI ⟶ Payment handling
8. **MVP-2.3**: Coupon Management UI ⟶ Marketing tool
9. **MVP-2.4**: Support Ticket System UI ⟶ Customer support
10. **MVP-3.1**: Content Management UI ⟶ User communication
11. **MVP-3.2**: Website Management UI ⟶ User content control
12. **MVP-3.3**: Feature Flag UI ⟶ Release management
13. **MVP-3.4**: System Health Dashboard ⟶ Operational visibility
14. **MVP-4.1**: Custom Report Builder ⟶ Advanced analytics
15. **MVP-4.2**: A/B Testing Dashboard ⟶ Optimization
16. **MVP-4.3**: AI Usage Analytics UI ⟶ Cost tracking
17. **MVP-4.4**: Automation Workflows UI ⟶ Efficiency
18. **MVP-4.5**: GDPR & Bulk Operations ⟶ Legal compliance
19. **MVP-4.6**: Admin Activity Log UI ⟶ Audit trail

---

## Service Dependencies

| Dependency Type | Service & MVP | Required For | Purpose |
|-----------------|---------------|--------------|---------|
| **Authentication** | Resume-SSO (MVP-1.4) | Epic 1.1: Admin Auth | Basic login functionality |
| **MFA Security** | Resume-SSO (MVP-4.3) | Epic 1.1: Admin Auth | Multi-factor authentication for admins |
| **Admin Operations** | Resume-Admin-API (MVP-1) | MVP-1: Admin Foundation | User management, analytics, templates |
| **Financial Operations** | Resume-Admin-API (MVP-2) | MVP-2: Financial UI | Billing, payments, coupons, support |
| **Content Management** | Resume-Admin-API (MVP-3) | MVP-3: Content UI | Content, websites, feature flags |
| **Advanced Features** | Resume-Admin-API (MVP-4) | MVP-4: Advanced UI | Reports, A/B testing, automation |

**Internal Component Dependencies**:
| This Component | Required For | Reason |
|----------------|--------------|---------|
| MVP-1.1 (Admin Auth) | All admin features | Authentication required for access |
| MVP-1.2 (Dashboard) | User experience | Primary landing page |
| MVP-1.3 (User Management) | User operations | Core admin functionality |
| MVP-2.1 (Financial Dashboard) | Revenue visibility | Financial overview required |
| MVP-2.4 (Support Tickets) | Customer support | Support operations interface |
| MVP-3.3 (Feature Flags) | Release management | Control feature rollouts |
| MVP-3.4 (System Health) | Operational awareness | Monitor system status |

---

## Testing Strategy

### Unit Tests
- React component rendering
- Form validation
- State management (Zustand stores)
- Chart rendering
- Table filtering and sorting

### Integration Tests
- Admin authentication flow
- User management operations
- Support ticket workflow
- Feature flag configuration
- Report generation

### E2E Tests (Playwright)
- Full admin journey: login (with MFA) → dashboard → manage user → suspend
- Support flow: view ticket → reply → close
- Financial flow: view subscription → cancel → refund
- Feature flag flow: create → configure → enable
- Report flow: build → preview → export

### Performance Tests
- Dashboard load time (< 2 seconds)
- Table pagination (< 300ms)
- Chart rendering (< 1 second)
- Search results (< 500ms)

### Security Tests
- Role-based access control
- Session timeout
- MFA enforcement
- XSS protection
- CSRF protection

---

## Performance Targets

- Dashboard load time: < 2s
- Search results: < 500ms
- Chart rendering: < 1s
- Table pagination: < 300ms
- API calls: < 200ms (p95)
- Lighthouse Performance Score: > 90
- Bundle size: < 300KB (initial, gzipped)

---

## Environment Variables

```bash
# API Configuration
NEXT_PUBLIC_ADMIN_API_URL=https://admin-api.resumebuilder.com
NEXT_PUBLIC_SSO_URL=https://sso.resumebuilder.com

# Admin Configuration
NEXT_PUBLIC_ADMIN_SESSION_TIMEOUT=1800000 # 30 minutes in ms

# Feature Flags (local overrides)
NEXT_PUBLIC_ENABLE_AI_ANALYTICS=true
NEXT_PUBLIC_ENABLE_AUTOMATION=true

# Analytics
NEXT_PUBLIC_GA_TRACKING_ID=<google-analytics-id>
```

---

## Success Metrics

- [ ] 100% test coverage for critical paths
- [ ] Lighthouse Performance Score > 90
- [ ] Dashboard loads in < 2 seconds
- [ ] All admin roles functional
- [ ] MFA enforced for all admins
- [ ] Zero unauthorized access
- [ ] Audit log captures all actions
- [ ] Real-time updates work correctly
- [ ] Mobile usage possible (tablet+)
