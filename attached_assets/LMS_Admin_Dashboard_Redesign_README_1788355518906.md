# LMS Admin Dashboard Re-Architecture & Design System Specification
**Project:** Next-Gen Enterprise LMS Admin Control Panel  
**Version:** 2.0.0-Prototype  
**Target Audience:** Product Managers, UI/UX Engineers, Frontend Developers  
**Design Philosophy:** Scalable Information Architecture, High-density Data Clarity, RTL-First, Clean Component Hierarchy.

---

## 1. Executive Summary & Design Rationale

The legacy LMS control panel suffered from high cognitive load due to an icon-only flat navigation bar (16+ disconnected icons), excessive screen fragmentation (deep nested screens for course details), and inefficient two-column split master-detail patterns with empty dead space.

This specification provides the blueprint to build a modern, scalable prototype featuring:
- **Grouped 2-Level Sidebar Navigation:** Collapsible accordion modules with labeled RTL typography and persistent badge indicators.
- **Unified Workspace Layout:** Persistent sticky entity headers with horizontal tabs for deep resource management (e.g., Course details, Content builder, Assessment, Students).
- **Modern Data Tables & Bulk Operations:** Filter toolbars, multi-attribute search, faceted dropdowns, pagination, and batch actions.
- **Design System Standards:** Accessible color tokens, typography scales, spacing units, and interactive states.
- **Full Prototype Mock Data & Schemas:** Ready-to-use JSON payloads for API mocking and frontend component staging.

---

## 2. Design System & Style Guide

### 2.1 Color Palette & Semantic Tokens

```
/* Base Neutrals (Slate) */
--color-bg-app:        #F8FAFC;  /* Slate-50  - Main Canvas Background */
--color-bg-surface:    #FFFFFF;  /* Pure White - Cards, Panels, Drawers */
--color-border-subtle: #E2E8F0;  /* Slate-200 - Borders, Dividers */
--color-border-hover:  #CBD5E1;  /* Slate-300 - Interactive Borders */

/* Text Hierarchy */
--color-text-primary:   #0F172A; /* Slate-900 - Headings & Body Primary */
--color-text-secondary: #475569; /* Slate-600 - Subtitles, Labels */
--color-text-muted:     #94A3B8; /* Slate-400 - Placeholders, Captions */

/* Brand & Accent (Deep Indigo / Primary Blue) */
--color-primary-50:    #EEF2FF;
--color-primary-100:   #E0E7FF;
--color-primary-500:   #4F46E5;
--color-primary-600:   #4338CA;  /* Main Brand Action Accent */
--color-primary-700:   #3730A3;

/* Semantic Status Badges */
--color-success-bg:    #ECFDF5;  /* Emerald-50 */
--color-success-text:  #047857;  /* Emerald-700 */
--color-warning-bg:    #FFFBEB;  /* Amber-50 */
--color-warning-text:  #B45309;  /* Amber-700 */
--color-danger-bg:     #FEF2F2;  /* Rose-50 */
--color-danger-text:   #B91C1C;  /* Rose-700 */
--color-info-bg:       #F0F9FF;  /* Sky-50 */
--color-info-text:     #0369A1;  /* Sky-700 */
```

### 2.2 Typography Scale (RTL First - Arabic & English)
- **Primary Font Family (Arabic):** `'IBM Plex Sans Arabic'`, `'Readex Pro'`, or `'Cairo'`, `sans-serif`
- **Primary Font Family (Latin / Monospace):** `'Inter'`, `'JetBrains Mono'` (for codes/IDs)

| Token | Size (px/rem) | Weight | Line Height | Use Case |
|---|---|---|---|---|
| `text-display` | 28px (1.75rem) | Bold (700) | 1.25 | Main Dashboard Summary Numbers |
| `text-h1` | 22px (1.375rem) | SemiBold (600) | 1.3 | Page Titles, Primary Section Headers |
| `text-h2` | 18px (1.125rem) | SemiBold (600) | 1.35 | Modal Headers, Tab Titles, Card Titles |
| `text-body` | 14px (0.875rem) | Regular (400) | 1.5 | Standard Table Rows, Inputs, Descriptions |
| `text-body-bold`| 14px (0.875rem) | Medium (500) | 1.5 | Table Headers, Navigation Items |
| `text-caption` | 12px (0.75rem) | Regular (400) | 1.4 | Badges, Timestamp Meta, Help Text |

### 2.3 Layout Grid & Sizing Principles
- **Sidebar Width (Expanded):** `260px` (RTL right-aligned)
- **Sidebar Width (Collapsed Icon-Only):** `72px`
- **Header Topbar Height:** `64px` (Sticky `top-0`, `z-index: 30`)
- **Content Area Max Width:** `1440px` centered or full fluid width with padding `px-6 py-6`
- **Border Radius:**
  - `rounded-sm`: 4px (Badges, small tags)
  - `rounded-md`: 8px (Form fields, buttons)
  - `rounded-lg`: 12px (Cards, Modals, Tab containers)
  - `rounded-xl`: 16px (Flyout drawers)

---

## 3. Navigation & Section Hierarchy (Information Architecture)

### 3.1 Sidebar Navigation Groupings

```
Root Sidebar
│
├── 📊 الرئيسية (Dashboard Overview)
│   └── نظرة عامة ومؤشرات الأداء (/dashboard)
│
├── 🎓 إدارة المحتوى التعليمي (Academic Content) [Collapsible Group]
│   ├── الدورات التدريبية (/courses)
│   ├── الباقات والعروض (/packages)
│   └── الاستشارات والجلسات (/consultations)
│
├── 💼 إدارة المبيعات والتسويق (Commercial & Sales) [Collapsible Group]
│   ├── المبيعات والطلبات (/sales)
│   ├── كوبونات الخصم (/coupons)
│   ├── المسوقين والعمولات (/affiliates)
│   └── بوابة الشركات B2B (/b2b)
│
├── 💬 خدمة العملاء والتفاعل (Support & Feedback) [Collapsible Group]
│   └── تذاكر الدعم الفني (/tickets) [Has Unread Counter: 14]
│
└── ⚙️ الإعدادات والنظام (System & Platform) [Collapsible Group]
    ├── سياسات وشروط البرنامج (/policies)
    └── الإعدادات العامة للمنصة (/settings)
```

### 3.2 Global Header (Top Bar)
- **Right Side (RTL):** Sidebar Toggle Button (Hamburger / Collapse Arrow) + Current Breadcrumbs (e.g., `الدورات التدريبية / دورة تحليل البيانات / المنهج والمحتوى`).
- **Center:** Quick Search Bar (`Cmd + K` spotlight launcher for fast jump to course, student, or ticket).
- **Left Side (RTL):**
  - Notification Bell with unread indicator badge.
  - Organization / B2B Branch Switcher dropdown.
  - Admin Profile Avatar with quick logout and settings menu.

---

## 4. Pages Structure, Workspaces & UI Components

### Page 1: Dashboard Home (الرئيسية)
- **KPI Metrics Cards:** 4 stats cards (Total Revenue, Active Students, Published Courses, Open Tickets) with percentage growth tags.
- **Recent Enrolments & Urgent Tickets:** 2-column split with latest orders and unassigned tickets.
- **Platform Health & Quick Actions:** Shortcut buttons for `+ دورة جديدة`, `+ كوبون خصم`, `+ عميل شركات جديد`.

### Page 2: Courses Management (الدورات التدريبية)
- **Master List View:**
  - **Filter Bar:** Search input, Status filter (`منشور`, `مسودة`, `مؤرشف`), Trainer dropdown, Price sorting.
  - **Data Table:** Course Name + Thumbnail, Trainer, Enrolled Students, Rating (e.g., ⭐ 4.8), Price (SAR), Status Badge, Action Menu (`تعديل`, `إدارة المحتوى`, `معاينة`, `حذف`).
- **Course Workspace (2.1 & 2.1.1):** When clicking a course, opens a **Tabbed Workspace**:
  - **Header:** Sticky banner with Title, Thumbnail, Category, Quick Stats (Revenue, Students), Save & Publish buttons.
  - **Tab 1: البيانات الأساسية (Basic Info):** Course Title, Description, Categories, Assigned Trainers (`المدربين`), Pricing, Course Images & Promo Video.
  - **Tab 2: المنهج والمحتوى (Curriculum Builder):**
    - Section Accordions (`الأقسام التدريبية`).
    - Nested Lesson items (Video, PDF, Live Stream `البث المباشر`, Announcements `الاعلانات`).
    - Drag & drop handles for section/lesson reordering.
  - **Tab 3: التقييم والأنشطة (Assessments & Activities):**
    - Quizzes (`الاختبارات`), Assignments (`الواجبات`), Interactive tasks (Sentence matching `ربط الجمل`).
    - Passing grade configurations and automated certificate assignment (`الشهادات`).
  - **Tab 4: المتدربون (Enrolled Learners):** Searchable table of registered students, completion rate, scores, manual certificate override.
  - **Tab 5: مبيعات الدورة (Sales & Coupons):** Ledger of all sales specific to this course.

### Page 3: Packages Management (الباقات التدريبية)
- Bundled course offerings with bundle pricing, thumbnail galleries, linked course tags, and validity periods.
- Single unified creation drawer: Select courses via tag selector, define bundle discount price, and upload promo assets.

### Page 4: Coupons Management (كوبونات الخصم)
- **Coupon Rule Engine:** Single screen replacing fragmented sub-pages.
- Code generator, Discount type (Percentage % vs Fixed SAR), Usage limits (Total / Per User), Validity Range (Start & End Date Pickers).
- Scope Selector: Applies to `All Courses`, `Specific Courses`, `Bundles`, or `Consultations`.

### Page 5: Consultations (الاستشارات وجلسات التدريب)
- Dedicated views for consultants, their available time slots, appointment duration, session pricing, and incoming booking calendar.

### Page 6 & 7: Affiliates & Commissions (إدارة المسوقين والعمولات)
- **Affiliates List:** Marketer Name, Unique Referral Code, Total Clicks, Successful Conversions, Total Commission Owed, Payment Status.
- **Payout Management Drawer:** Review pending commission withdrawals, approve/reject with transaction reference upload.

### Page 8: B2B Corporate Portal (بوابة الشركات)
- **Kanban Pipeline & Table Toggle:**
  - Column 1: `طلبات جديدة (New Inquiries)`
  - Column 2: `تم التواصل (Contacted)`
  - Column 3: `إدارة الاشتراكات المفعلة (Active Subscriptions)`
  - Column 4: `طلبات الاستشارة (Consultation Requests)`
- Single-click export of enterprise roster to Excel.

### Page 9: Tickets & Support (تذاكر الدعم الفني)
- **Dual-Pane Zendesk/Slack Style Layout:**
  - **Right Column (List):** Ticket feed filtered by Status (`مفتوحة`, `قيد المعالجة`, `مغلقة`) and Priority (`عالية`, `متوسطة`).
  - **Left Area (Conversation Canvas):** Message history timeline, internal team notes, quick response snippets (Canned responses), and user sidebar context.

---

## 5. Comprehensive Mock Data (JSON Schemas)

Use these mock datasets in your state management store (Pinia / Redux / Zustand) or mock API handlers (MSW / MirageJS) to build the prototype.

```json
{
  "dashboardStats": {
    "totalRevenue": 485250,
    "revenueCurrency": "SAR",
    "growthRate": 14.2,
    "activeLearners": 12840,
    "totalCourses": 64,
    "pendingTickets": 14
  },
  "courses": [
    {
      "id": "CRS-101",
      "title": "تحليل البيانات واستخراج الأنماط باستخدام الذكاء الاصطناعي",
      "slug": "ai-data-analysis",
      "status": "published",
      "category": "الذكاء الاصطناعي وتحليل البيانات",
      "price": 850,
      "currency": "SAR",
      "thumbnail": "https://images.unsplash.com/photo-1551288049-bebda4e38f71?w=400",
      "trainers": [
        { "id": "TRN-01", "name": "د. مظفر قنطقجي", "avatar": "https://i.pravatar.cc/150?u=1" },
        { "id": "TRN-02", "name": "م. ثابت حجازي", "avatar": "https://i.pravatar.cc/150?u=2" }
      ],
      "stats": {
        "enrolledStudents": 420,
        "completionRate": 78.5,
        "averageRating": 4.9,
        "reviewCount": 112,
        "totalRevenue": 357000
      },
      "curriculumSections": [
        {
          "id": "SEC-01",
          "order": 1,
          "title": "المقدمة والأساسيات المعرفية",
          "items": [
            { "id": "ITM-01", "type": "video", "title": "مقدمة البرنامج وأهداف التدريب", "duration": "14:30" },
            { "id": "ITM-02", "type": "document", "title": "حقيبة المتدرب ومصادر الدورة PDF", "fileSize": "4.2 MB" },
            { "id": "ITM-03", "type": "announcement", "title": "رابط قناة التليجرام الخاصة بنقاشات الدورة" }
          ]
        },
        {
          "id": "SEC-02",
          "order": 2,
          "title": "ورش العمل التفاعلية والبث المباشر",
          "items": [
            { "id": "ITM-04", "type": "live_stream", "title": "جلسة مباشرة: تطبيقات بايثون في التنقيب عن البيانات", "date": "2026-09-15T18:00:00Z", "status": "scheduled" },
            { "id": "ITM-05", "type": "assignment", "title": "واجب رقم 1: تنظيف مجموعة بيانات المبيعات", "dueDays": 5 },
            { "id": "ITM-06", "type": "activity_match", "title": "نشاط تفاعلي: ربط الخوارزميات بالاستخدام المناسب" }
          ]
        },
        {
          "id": "SEC-03",
          "order": 3,
          "title": "التقييم الختامي والاعتماد",
          "items": [
            { "id": "ITM-07", "type": "quiz", "title": "الاختبار النهائي للدورة (30 سؤال)", "passingScore": 75 },
            { "id": "ITM-08", "type": "certificate", "title": "إصدار الشهادة المعتمدة تلقائياً عند الإتمام" }
          ]
        }
      ]
    },
    {
      "id": "CRS-102",
      "title": "استراتيجيات نجاح الشركات العائلية وضمان استدامتها",
      "slug": "family-business-strategy",
      "status": "draft",
      "category": "إدارة الأعمال والقيادة",
      "price": 1200,
      "currency": "SAR",
      "thumbnail": "https://images.unsplash.com/photo-1486406146926-c627a92ad1ab?w=400",
      "trainers": [
        { "id": "TRN-03", "name": "سهل مهدي", "avatar": "https://i.pravatar.cc/150?u=3" }
      ],
      "stats": {
        "enrolledStudents": 0,
        "completionRate": 0,
        "averageRating": 0,
        "reviewCount": 0,
        "totalRevenue": 0
      },
      "curriculumSections": []
    }
  ],
  "packages": [
    {
      "id": "PKG-01",
      "name": "باقة التسويق والتجارة الإلكترونية الشاملة",
      "status": "active",
      "originalPrice": 2400,
      "bundlePrice": 1499,
      "currency": "SAR",
      "enrolledCount": 119,
      "totalRevenue": 178381,
      "includedCourses": ["CRS-101", "CRS-105", "CRS-108"],
      "createdAt": "2026-04-10"
    }
  ],
  "coupons": [
    {
      "id": "CPN-01",
      "code": "SUMMER2026",
      "discountType": "percentage",
      "value": 25,
      "scope": "all_courses",
      "usageCount": 142,
      "maxUsage": 500,
      "startDate": "2026-06-01",
      "endDate": "2026-09-30",
      "status": "active"
    },
    {
      "id": "CPN-02",
      "code": "B2B-VIP",
      "discountType": "fixed",
      "value": 200,
      "scope": "packages",
      "usageCount": 18,
      "maxUsage": 50,
      "startDate": "2026-08-01",
      "endDate": "2026-10-31",
      "status": "active"
    }
  ],
  "b2bRequests": [
    {
      "id": "B2B-501",
      "companyName": "شركة الدكتور للاستثمار والتطوير العقاري",
      "contactPerson": "طرق محمد",
      "contactPhone": "+96879065940",
      "requestedSeats": 45,
      "assignedPackage": "باقة التسويق والتجارة الإلكترونية",
      "stage": "new_request",
      "createdAt": "2026-08-28"
    },
    {
      "id": "B2B-502",
      "companyName": "مفارش لازيرا هوم",
      "contactPerson": "عبدالعزيز الغامدي",
      "contactPhone": "+966581111595",
      "requestedSeats": 120,
      "assignedPackage": "برنامج مهارات المبيعات الاحترافية",
      "stage": "contacted",
      "createdAt": "2026-08-24"
    },
    {
      "id": "B2B-503",
      "companyName": "شركة سمارت بان للحلول البرمجية",
      "contactPerson": "جعفر السقاف",
      "contactPhone": "+966504300370",
      "requestedSeats": 80,
      "assignedPackage": "مسار الذكاء الاصطناعي وتحليل البيانات",
      "stage": "active_subscription",
      "createdAt": "2026-07-15"
    }
  ],
  "affiliates": [
    {
      "id": "AFF-201",
      "name": "زايد أحمد أبو زايد",
      "email": "zaidahmedabuzaid6@gmail.com",
      "referralCode": "ZAID2026",
      "status": "active",
      "totalConversions": 84,
      "totalEarnedCommission": 9450,
      "pendingPayout": 2100,
      "paymentMethod": "Bank Transfer (Al Rajhi Bank)"
    },
    {
      "id": "AFF-202",
      "name": "محسنة حسين أحمد الفيفي",
      "email": "mhayas2009@gmail.com",
      "referralCode": "HAYAS10",
      "status": "active",
      "totalConversions": 19,
      "totalEarnedCommission": 1710,
      "pendingPayout": 0,
      "paymentMethod": "STC Pay"
    }
  ],
  "tickets": [
    {
      "id": "TCK-8801",
      "title": "ربط الإيميل برقم الموظف بالشركة",
      "user": {
        "name": "فيصل القصبي",
        "email": "f.algasabi@gmail.com",
        "phone": "+966558882346",
        "company": "Atmaal",
        "employeeId": "500774"
      },
      "category": "حسابات الشركات B2B",
      "priority": "high",
      "status": "open",
      "createdAt": "2026-09-02T09:14:00Z",
      "messages": [
        {
          "sender": "user",
          "senderName": "فيصل القصبي",
          "timestamp": "2026-09-02T09:14:00Z",
          "content": "السلام عليكم، تم إكمال الدورات الإلزامية للموظف وأرغب بربط حسابي برقم الموظف لإشعار الشركة بذلك. رقم الموظف: TN500774 ولكم الشكر والتقدير."
        }
      ]
    },
    {
      "id": "TCK-8802",
      "title": "أنهيت دورة التوعية بالاحتيال لكن لا يوجد خيار لتنزيل الشهادة",
      "user": {
        "name": "تغريد الحربي",
        "email": "alharbi.f.taghreed@gmail.com",
        "phone": "+966502907811",
        "company": "Atmaal",
        "employeeId": "33555"
      },
      "category": "الشهادات والاعتمادات",
      "priority": "medium",
      "status": "open",
      "createdAt": "2026-09-01T15:20:00Z",
      "messages": [
        {
          "sender": "user",
          "senderName": "تغريد الحربي",
          "timestamp": "2026-09-01T15:20:00Z",
          "content": "أنهيت جميع مقاطع الدورة والاختبار بنسبة 95% ولكن خانة تنزيل الشهادة لا زالت مقفلة وغير نشطة. يرجى تفعيلها."
        }
      ]
    }
  ]
}
```

---

## 6. Implementation Checklist & Development Steps

Follow this phased checklist to implement the prototype:

1. **Step 1: Setup Core Layout Shell**
   - Implement the collapsible 2-level sidebar supporting full RTL layout (`dir="rtl"`).
   - Configure Tailwind CSS theme tokens for fonts, colors, and shadows matching Section 2.
   - Build the sticky top bar with breadcrumb tracking and global search bar.

2. **Step 2: Generic Table & Filter Component**
   - Create a reusable `<AdminDataTable>` component featuring:
     - Search input with debounce.
     - Faceted dropdown filters (Category, Status, Trainer).
     - Standard pagination footer (rows per page, total record count, next/previous).
     - Batch selection with top action bar (Delete selected, Export).

3. **Step 3: Course Workspace (`/courses/:id`)**
   - Implement the Sticky Header containing quick stats and action buttons (`حفظ كمسودة`, `نشر الدورة`).
   - Implement the 5-Tab interface with smooth transitions.
   - Build the Accordion-based Curriculum Builder supporting drag-and-drop handles for modules and lessons.

4. **Step 4: Support Tickets Split Workspace (`/tickets`)**
   - Implement the 2-pane master-detail ticket console.
   - Left pane: Message timeline + reply box with canned response selector.
   - Right pane: Customer context sidebar with user attributes and enterprise company link.

5. **Step 5: B2B CRM Pipeline (`/b2b`)**
   - Build the Stage Board (Kanban) with columns: `الطلبات الجديدة`, `تم التواصل`, `الاشتراكات المفعلة`.
   - Provide a view-toggle button between Kanban cards and full Data Table view.
