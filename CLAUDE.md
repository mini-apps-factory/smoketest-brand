# SmokeTest Brand

## PROJECT
- Category: E-Commerce / Marketplace
- Tier: MVP
- Slug: smoketest-brand
- Generated: 2026-04-27

## SCREENS (10)
1. Splash Screen: Brand logo and loading state while checking user authentication
2. Product Catalog: Grid/list view of products with search, filters, categories. User can browse and tap products
3. Product Detail: Full product info, images, price, description, variants, add to cart button
4. Shopping Cart: List of selected items, quantities, total price, remove items, proceed to checkout
5. Checkout: Delivery address form, contact info, delivery method selection
6. Payment: Payment method selection and Telegram payment processing
7. Order Success: Confirmation message, order number, delivery estimate, view order details button
8. My Orders: List of user's orders with status, date, total. Tap to view details
9. Order Detail: Full order information, items, status tracking, delivery info, support contact
10. Profile: User info, saved addresses, settings, logout option

## ROLES
- customer: End user who browses products, makes purchases, and tracks orders
- admin: Brand administrator who monitors orders and customer activity

## NAVIGATION
- Tab Bar: Catalog, Cart, Orders, Profile
- Stack: product_detail, checkout, payment, order_detail
- Modal: order_success

## USER FLOWS
1. Product Purchase Flow (customer): catalog → product_detail → cart → checkout → payment → order_success → orders
2. Order Tracking Flow (customer): orders → order_detail
3. Browse and Search Flow (customer): catalog → product_detail → catalog or cart

## DATABASE (11 tables)
### users
id uuid, telegram_id bigint, username text, first_name text, last_name text, phone text, photo_url text, user_type text, created_at timestamptz, updated_at timestamptz

### specialist_profiles
id uuid, user_id uuid, bio text, years_experience integer, certifications text[], average_rating decimal(3,2), total_reviews integer, completed_jobs integer, response_time_minutes integer, is_active boolean, accepting_bookings boolean, location_lat decimal(10,8), location_lng decimal(11,8), location_address text, created_at timestamptz, updated_at timestamptz

### services
id uuid, specialist_id uuid, category text, name text, description text, price_type text, price decimal(10,2), duration_minutes integer, is_active boolean, created_at timestamptz

### portfolio_images
id uuid, specialist_id uuid, image_url text, display_order integer, created_at timestamptz

### availability_schedules
id uuid, specialist_id uuid, day_of_week integer, start_time time, end_time time, is_available boolean, created_at timestamptz

### blocked_dates
id uuid, specialist_id uuid, blocked_date date, start_time time, end_time time, reason text, created_at timestamptz

### bookings
id uuid, reference_number text, client_id uuid, specialist_id uuid, service_id uuid, booking_date date, start_time time, end_time time, service_address text, client_notes text, status text, service_price decimal(10,2), platform_fee decimal(10,2), total_amount decimal(10,2), payment_status text, payment_transaction_id text, decline_reason text, cancelled_by uuid, cancelled_at timestamptz, completed_at timestamptz, created_at timestamptz, updated_at timestamptz

### reviews
id uuid, booking_id uuid, specialist_id uuid, client_id uuid, rating integer, review_text text, photo_urls text[], created_at timestamptz

### payments
id uuid, booking_id uuid, specialist_id uuid, transaction_id text, amount decimal(10,2), platform_commission decimal(10,2), specialist_payout decimal(10,2), payment_method text, status text, payout_date date, created_at timestamptz, updated_at timestamptz

### notifications
id uuid, user_id uuid, type text, title text, body text, data jsonb, is_read boolean, deep_link text, created_at timestamptz

### notification_settings
id uuid, user_id uuid, booking_accepted boolean, booking_declined boolean, booking_reminder boolean, job_completed boolean, review_reminder boolean, new_message boolean, new_booking_request boolean, payment_received boolean, dnd_start_time time, dnd_end_time time, created_at timestamptz, updated_at timestamptz

## API ENDPOINTS (32)
- GET /api/specialists: List specialists with filters (service_type, availability, rating, price_range, location)
- GET /api/specialists/:id: Get specialist profile details with services, reviews, and availability
- POST /api/specialists: Create specialist profile
- PUT /api/specialists/:id: Update specialist profile
- GET /api/specialists/:id/availability: Get specialist availability for date range
- PUT /api/specialists/:id/availability: Update specialist availability schedule and blocked dates
- GET /api/specialists/:id/services: Get specialist services list
- POST /api/specialists/:id/services: Add service to specialist profile
- PUT /api/services/:id: Update service details and pricing
- DELETE /api/services/:id: Delete service
- POST /api/specialists/:id/portfolio: Upload portfolio image
- DELETE /api/portfolio/:id: Delete portfolio image
- GET /api/specialists/:id/reviews: Get specialist reviews with pagination
- POST /api/bookings: Create new booking request
- GET /api/bookings/:id: Get booking details
- PUT /api/bookings/:id/accept: Specialist accepts booking request
- PUT /api/bookings/:id/decline: Specialist declines booking request
- PUT /api/bookings/:id/cancel: Cancel booking (client or specialist)
- PUT /api/bookings/:id/complete: Mark booking as completed (specialist only)
- GET /api/bookings: List user bookings with filters (status, date_range)
- POST /api/payments/initiate: Initiate payment for booking via Telegram Payment API
- POST /api/payments/webhook: Handle Telegram payment webhook
- GET /api/payments/:booking_id/receipt: Get payment receipt for booking
- POST /api/reviews: Submit review for completed booking
- GET /api/specialists/:id/earnings: Get specialist earnings dashboard data
- GET /api/specialists/:id/statistics: Get specialist statistics (completed jobs, ratings, etc.)
- GET /api/notifications: Get user notifications
- PUT /api/notifications/:id/read: Mark notification as read
- GET /api/notification-settings: Get user notification settings
- PUT /api/notification-settings: Update notification settings
- GET /api/user/profile: Get current user profile
- PUT /api/user/profile: Update user profile

## MODULES (5)
### core
- src/app/layout.tsx
- src/app/page.tsx
- src/providers/TelegramProvider.tsx
- src/providers/SupabaseProvider.tsx
- src/components/layout/ErrorBoundary.tsx
- src/components/layout/LoadingSpinner.tsx

### auth
- src/app/auth/login/page.tsx
- src/hooks/useAuth.ts
- src/components/features/auth/TelegramLoginButton.tsx
- src/components/features/auth/ProtectedRoute.tsx

### common
- src/components/common/Button.tsx
- src/components/common/Card.tsx
- src/components/common/Avatar.tsx
- src/components/common/EmptyState.tsx
- src/components/common/ErrorState.tsx
- src/utils/format.ts
- src/utils/api.ts
- src/utils/validation.ts

### navigation
- src/components/layout/BottomNav.tsx
- src/components/layout/Header.tsx
- src/hooks/useTelegram.ts

### settings
- src/app/settings/page.tsx
- src/components/features/settings/SettingsList.tsx
- src/components/features/settings/SettingItem.tsx
- src/hooks/useSettings.ts

## TECH STACK
- Framework: Next.js 14 (App Router)
- Language: TypeScript (strict)
- Styling: TailwindCSS + shadcn/ui
- Backend: Supabase (PostgreSQL + Auth + Storage + Realtime)
- Hosting: Vercel
- Platform: Telegram Mini App SDK (@telegram-apps/sdk-react)

## DESIGN KIT v1.1

### Color Tokens
```
--color-primary: #2563EB
--color-secondary: #059669
--color-accent: #F59E0B
--color-background: #FFFFFF
--color-surface: #F8FAFC
--color-text: #0F172A
--color-text-secondary: #64748B
--color-error: #EF4444
--color-success: #22C55E
--color-warning: #F59E0B
```

### Spacing Scale
```
--space-1: 4px    --space-2: 8px    --space-3: 12px
--space-4: 16px   --space-5: 20px   --space-6: 24px
--space-8: 32px   --space-10: 40px  --space-12: 48px
```

### Border Radius
```
--radius-sm: 6px
--radius-md: 12px
--radius-lg: 16px
--radius-full: 9999px
```

### Typography
```
--font-family: Inter, -apple-system, sans-serif
--font-h1: 24px/32px bold
--font-h2: 20px/28px semibold
--font-h3: 16px/24px semibold
--font-body: 16px/24px regular
--font-caption: 12px/16px regular
--font-button: 14px/20px medium
```

### Shadows
```
--shadow-sm: 0 1px 2px rgba(0,0,0,0.05)
--shadow-card: 0 2px 8px rgba(0,0,0,0.08)
--shadow-modal: 0 8px 32px rgba(0,0,0,0.12)
--shadow-bottom-bar: 0 -2px 8px rgba(0,0,0,0.06)
```

### Component Library (shadcn/ui)
- Button: primary, secondary, outline, ghost, destructive
- Card: default, interactive (hover:shadow-md, tap:scale(0.98))
- Input: default, error, disabled
- Badge: status (pending=yellow, active=green, cancelled=red)
- Skeleton: card, list-item, text-block (pulse 1.5s)
- EmptyState: icon + title + subtitle + action
- ErrorState: icon + message + retry button
- BottomSheet: modal with drag handle (slide-up 300ms)
- TabBar: max 5 items, icons + labels, safe-area padding
- Toast: slide-down + fade, auto-dismiss 3s

## MOTION LIBRARY

### Transition Defaults
```
--duration-fast: 150ms
--duration-normal: 250ms
--duration-slow: 350ms
--easing-default: cubic-bezier(0.4, 0, 0.2, 1)
--easing-spring: cubic-bezier(0.34, 1.56, 0.64, 1)
--easing-ease-out: cubic-bezier(0, 0, 0.2, 1)
```

### Page Transitions
```
push: slide-in-right 250ms ease-out
pop: slide-out-right 200ms ease-out
modal-open: slide-up 300ms spring + backdrop fade-in 200ms
modal-close: slide-down 250ms ease-out + backdrop fade-out 150ms
tab-switch: crossfade 150ms default
```

### Component Animations
```
button-press: scale(0.97) 100ms -> scale(1) 150ms spring + haptic
card-tap: scale(0.98) 150ms ease-out
list-enter: fade-in + slide-up 250ms (stagger 50ms per item, max 8)
skeleton-pulse: opacity 0.4 -> 1.0, 1.5s ease-in-out infinite
toast-in: slide-down 250ms spring + fade-in
toast-out: fade-out 200ms + slide-up
pull-to-refresh: custom spinner, 60px threshold, rubber-band 300ms
fab-press: scale(0.9) 100ms -> scale(1) 200ms spring
bottom-sheet: slide-up 300ms spring, dismiss velocity > 500px/s
```

### Haptic Feedback (Telegram)
```
button-tap: impact light
success-action: notification success
error-action: notification error
toggle-switch: impact medium
selection-change: selection changed
long-press: impact heavy
pull-refresh: impact light (at threshold)
```

### Gesture Thresholds
```
swipe-dismiss: velocity > 500px/s OR distance > 40% screen
pull-to-refresh: 60px pull distance
long-press: 500ms hold
scroll-snap: nearest item at deceleration
```

## CODING RULES

### Architecture
1. "use client" ONLY where useState/useEffect/events needed
2. Server Components by default
3. One component per file, named export = filename
4. Max 200 lines per component — split if larger
5. Barrel exports via index.ts per module

### File Organization
6. Pages: src/app/(tabs)/[feature]/page.tsx
7. Components: src/components/features/[feature]/ComponentName.tsx
8. Shared: src/components/ui/ (shadcn primitives only)
9. Hooks: src/hooks/use[Name].ts
10. Types: src/types/[domain].ts
11. Utils: src/lib/[name].ts
12. Constants: src/constants/[name].ts

### Supabase
13. createBrowserClient from @supabase/ssr (NOT createClient)
14. NEVER expose service_role key in client code
15. All queries via RLS — no service_role bypass in frontend
16. Use .single() for single-record queries
17. Error pattern: const { data, error } = await ...; if (error) throw error;
18. Realtime: subscribe to specific table changes, cleanup on unmount

### TypeScript
19. Strict mode ON, no `any`, no @ts-ignore, no @ts-expect-error
20. Interfaces for all: API responses, DB rows, component props
21. Zod schemas for runtime validation of external data
22. Prefer const assertions and discriminated unions
23. Generic types for reusable patterns: ApiResponse<T>, PaginatedList<T>

### Styling
24. TailwindCSS ONLY — no inline styles, no CSS modules, no styled-components
25. Design Kit tokens via CSS custom properties (var(--color-primary))
26. Mobile-first responsive: base -> sm: -> md: -> lg:
27. cn() helper from lib/utils for conditional classes
28. Telegram safe areas: env(safe-area-inset-top/bottom)
29. Dark mode: respect Telegram theme via CSS vars

### Components
30. Loading = Skeleton (NEVER show blank screen or spinner)
31. Empty = EmptyState component with illustration + title + CTA
32. Error = ErrorBoundary at route level, ErrorState for data fetch errors
33. Motion Library transitions for ALL state changes and navigations
34. Haptic feedback on EVERY interactive element (buttons, toggles, cards)
35. Optimistic updates for user actions (show immediately, sync in background)

### Performance
36. next/image with lazy loading, WebP format, blur placeholder
37. Virtualize lists >50 items (react-window or @tanstack/virtual)
38. Debounce search input 300ms
39. Prefetch adjacent routes on hover/focus
40. Bundle analysis: no single chunk > 200KB

### Testing
41. Utils/hooks: Vitest unit tests
42. Components: React Testing Library
43. Critical user flows: Playwright E2E
44. Coverage target: 80% for utils, 60% for components

## DEPENDENCIES

### REQUIRED (always install)
next, react, react-dom, @supabase/supabase-js, @supabase/ssr,
tailwindcss, @telegram-apps/sdk-react, lucide-react, zod,
class-variance-authority, clsx, tailwind-merge, framer-motion

### OPTIONAL (install when needed)
react-window (lists >50), @tanstack/react-query (complex data),
date-fns (dates), recharts (charts), react-hook-form (forms)

### DO NOT ADD (banned)
axios, styled-components, redux, moment.js, lodash (use native),
emotion, material-ui, ant-design, jquery, underscore

## ENV VARS
```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
NEXT_PUBLIC_TELEGRAM_BOT_USERNAME=
TELEGRAM_BOT_TOKEN=
NEXT_PUBLIC_APP_URL=
```

## SQL SCHEMA
```sql
CREATE TABLE IF NOT EXISTS users (id UUID PRIMARY KEY DEFAULT gen_random_uuid(), telegram_id BIGINT UNIQUE NOT NULL, username TEXT, first_name TEXT, last_name TEXT, photo_url TEXT, created_at TIMESTAMPTZ DEFAULT NOW(), updated_at TIMESTAMPTZ DEFAULT NOW()); CREATE INDEX idx_users_telegram_id ON users(telegram_id); ALTER TABLE users ENABLE ROW LEVEL SECURITY; CREATE POLICY "Users can view own data" ON users FOR SELECT USING (auth.uid() = id); CREATE POLICY "Users can update own data" ON users FOR UPDATE USING (auth.uid() = id);
```

## ARKA SCORE
- Total: 58/100
