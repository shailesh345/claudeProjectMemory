# Phase 1: Discovery & Opportunity Finding

## 1.1 Browse Campaign Marketplace

### Route
**Primary**: `/app/marketplace`

### Campaign Tabs

**Tab Views:**
1. **For You** - AI-recommended campaigns based on:
   - Your profile and niche
   - Audience demographics
   - Past performance
   - Engagement rate
   - Follower count

2. **Trending** - Most popular campaigns with highest application volume

3. **Highest Pay** - Campaigns sorted by budget (descending)

4. **Closing Soon** - Campaigns with approaching application deadlines

### Campaign Card Information

**Each Card Shows:**
- Campaign title and description
- Brand name and logo
- Budget range ($500-$1,000)
- Target platform (Instagram, YouTube, TikTok, etc.)
- Content type (Post, Reel, Story, Video, etc.)
- Spots available / spots filled (e.g., "3/10 filled")
- Application deadline
- AI Match Score (compatibility percentage: 85% match)

### Filtering Options

**Filter By:**
- **Platform**: Instagram, YouTube, TikTok, Twitter, Facebook
- **Budget Range**: Minimum - Maximum sliders
- **Niche/Category**: Your content categories
- **Content Type**: Post, Reel, Story, Video, etc.
- **Campaign Duration**: Short (1-7 days), Medium (1-4 weeks), Long (1-3 months)
- **Location Requirements**: Local, National, Global

### Actions on Each Campaign

- 👁️ **View Details**: Full campaign information
- ⚡ **Quick Apply**: One-click application
- 💾 **Save**: Bookmark for later (stored via `/api/v1/bookmarks`)
- 🤖 **AI Match Score**: See compatibility rating

### AI Match Scoring

**Backend:**
- `/api/v1/matching` - Rule-based matching
- `/api/v1/ai-matching` - AI-powered recommendations

**Scoring Factors:**
- Niche alignment (does your content match campaign category?)
- Audience demographics (does your audience match brand's target?)
- Engagement rate (is your engagement rate sufficient?)
- Past campaign success (have you done similar campaigns well?)
- Rate compatibility (does budget match your rate card?)

**Recommended Campaigns API:**
- `GET /api/v1/marketplace/recommended` - AI-curated campaigns for you

## 1.2 Monitor Direct Offers

### Route
**Primary**: `/app/direct-offers`

### How Brands Find You

**Discovery Channels:**
1. Via `/app/tools/discovery` - Creator Discovery Page
2. Via AI matching recommendations
3. Via your public media kit URL
4. Via Spotlight promotions (if active)
5. Via search results (`/app/search`)

### Offer Tabs

**Tab Organization:**
- **Pending**: New offers awaiting your response (action required)
- **Negotiating**: Offers in counter-offer exchange
- **Accepted**: Offers you've accepted (converted to contracts)
- **Rejected**: Offers you've declined
- **Expired**: Offers that timed out (7-day default expiry)

### Offer Card Information

**Each Offer Shows:**
- Brand name and profile (with logo, verified badge)
- Offer title and description
- Budget amount ($X total)
- Deliverables breakdown:
  - Content types (2 Posts, 3 Stories, 1 Reel)
  - Quantities
  - Platforms
- Timeline:
  - Start date
  - Content submission deadline
  - Publishing date
  - End date
- Brand's personal message (why they chose you)
- Days remaining before expiry (countdown timer)
- AI match score (compatibility)

### Notifications

**Notification Channels:**
- **In-app notification**: Red badge with count
- **Email notification**: Instant email alert
- **Push notification**: Browser/mobile push
- **Badge count**: On notifications icon

**Backend**: `/api/v1/notifications`

### Offer Status Lifecycle
```
PENDING → VIEWED → ACCEPTED → CONVERTED_TO_CONTRACT
                 → REJECTED
                 → NEGOTIATING → (ACCEPTED / REJECTED)
                 → EXPIRED (after 7 days)
```

## 1.3 Boost Visibility with Spotlight

### Route
**Primary**: `/app/tools/spotlight`

### Spotlight Components

**1. SpotlightPromotion**
- Select promotion tier and pricing
- Choose promotion duration (7 days, 14 days, 30 days)
- Set budget for promotion

**2. SpotlightAnalytics**
- Track promotion performance
- Metrics tracked:
  - Profile views generated
  - Brand inquiries received
  - Campaign invitations received
  - Conversion rate (views → inquiries)
  - ROI of promotion

**3. TargetAudience**
- Set targeting preferences for which brands see your promotion
- Filters:
  - Industry (Fashion, Tech, Beauty, etc.)
  - Company size (Startup, SMB, Enterprise)
  - Budget range (brands with $X-$Y budgets)
  - Location (brands in specific regions)

### Process

1. Choose promotion tier:
   - **Basic**: $50/week - 1000 impressions
   - **Standard**: $150/week - 5000 impressions
   - **Premium**: $500/week - 20000 impressions

2. Set targeting preferences (industry, budget, location)

3. Pay for promotion (Stripe payment)

4. Promotion goes live - appears in brand discovery feeds

5. Monitor analytics dashboard:
   - Views today
   - Inquiries this week
   - Campaigns won from spotlight
   - Total ROI

6. Track inbound inquiries generated

## 1.4 Leverage AI Tools for Opportunity Preparation

### AI-Powered Tools

| Tool | Route / API | Purpose |
|------|-------------|---------|
| **AI Studio** | `/app/tools/ai-studio` | AI-powered content generation and planning |
| **Content Ideas** | `/api/v1/content-ideas` | Get AI suggestions aligned with trending campaigns |
| **Best Time to Post** | `/api/v1/best-time` | Optimal posting time per platform |
| **Pitch Writer** | `/api/v1/pitch-writer` | AI-assisted brand pitch writing for applications |
| **Revenue Optimization** | `/api/v1/revenue-optimization` | Tips to maximize earnings |
| **Market Rates** | `/api/v1/market-rates` | Benchmark your rates against similar creators |
| **Rising Stars** | `/api/v1/rising-stars` | Fast-growing creator detection (increases visibility) |

### AI Studio Usage

**Content Generation:**
- Generate campaign pitch messages
- Write video scripts
- Create caption ideas
- Develop content hooks
- Refine messaging to match brand tone

**Use Cases:**
- Before applying: Generate personalized pitch using AI Pitch Writer
- During discovery: Use Content Ideas to see what's trending
- Rate setting: Use Market Rates to benchmark pricing
- Profile optimization: Use Revenue Optimization for tips

## Database Models (Inferred)

### Campaign Model
```
Campaign {
  id: uuid
  brandId: foreign key
  title: string
  description: text
  budget: decimal
  budgetMin: decimal
  budgetMax: decimal
  platform: enum (INSTAGRAM, YOUTUBE, TIKTOK, etc.)
  contentType: array (POST, REEL, STORY, VIDEO)
  niche: array (categories)
  spotsTotal: integer
  spotsFilled: integer
  applicationDeadline: timestamp
  status: enum (DRAFT, OPEN, CLOSED, IN_PROGRESS, COMPLETED)
  createdAt: timestamp
}
```

### DirectOffer Model
```
DirectOffer {
  id: uuid
  brandId: foreign key
  creatorId: foreign key
  offerTitle: string
  description: text
  budget: decimal
  deliverables: JSON {
    posts: integer
    stories: integer
    reels: integer
    videos: integer
  }
  timeline: JSON {
    startDate: date
    submissionDeadline: date
    publishDate: date
    endDate: date
  }
  personalMessage: text
  status: enum (PENDING, VIEWED, NEGOTIATING, ACCEPTED, REJECTED, EXPIRED, CONVERTED_TO_CONTRACT)
  negotiationRound: integer
  expiresAt: timestamp (default: 7 days from creation)
  createdAt: timestamp
}
```

### Bookmark Model
```
Bookmark {
  id: uuid
  userId: foreign key (creator)
  campaignId: foreign key
  notes: text (optional personal notes)
  createdAt: timestamp
}
```

### SpotlightPromotion Model
```
SpotlightPromotion {
  id: uuid
  creatorId: foreign key
  tier: enum (BASIC, STANDARD, PREMIUM)
  duration: integer (days)
  budget: decimal
  targeting: JSON {
    industries: array
    companySize: array
    budgetRange: { min, max }
    locations: array
  }
  impressions: integer (delivered)
  views: integer
  inquiries: integer
  conversions: integer (campaigns won)
  startDate: timestamp
  endDate: timestamp
  status: enum (ACTIVE, COMPLETED, CANCELLED)
}
```

### AIMatchScore Model
```
AIMatchScore {
  campaignId: foreign key
  creatorId: foreign key
  overallScore: float (0-100)
  nicheAlignment: float
  audienceFit: float
  engagementMatch: float
  budgetCompatibility: float
  successPrediction: float
  calculatedAt: timestamp
}
```

## API Endpoints (Inferred)

### Marketplace
- `GET /api/v1/marketplace` - Get all campaigns
- `GET /api/v1/marketplace/for-you` - AI-recommended campaigns
- `GET /api/v1/marketplace/trending` - Trending campaigns
- `GET /api/v1/marketplace/highest-pay` - Sorted by budget desc
- `GET /api/v1/marketplace/closing-soon` - Sorted by deadline asc
- `GET /api/v1/marketplace/:id` - Get campaign details
- `POST /api/v1/marketplace/filter` - Apply filters
- `GET /api/v1/marketplace/recommended` - AI curated recommendations

### Direct Offers
- `GET /api/v1/direct-offers` - Get all offers
- `GET /api/v1/direct-offers/:id` - Get offer details
- `PATCH /api/v1/direct-offers/:id/view` - Mark as viewed
- `PATCH /api/v1/direct-offers/:id/accept` - Accept offer
- `PATCH /api/v1/direct-offers/:id/reject` - Reject offer
- `PATCH /api/v1/direct-offers/:id/counter` - Send counter-offer

### Bookmarks
- `GET /api/v1/bookmarks` - Get saved campaigns
- `POST /api/v1/bookmarks` - Save campaign
- `DELETE /api/v1/bookmarks/:id` - Remove bookmark
- `PUT /api/v1/bookmarks/:id/notes` - Update notes

### Spotlight
- `POST /api/v1/spotlight/promote` - Start promotion
- `GET /api/v1/spotlight/analytics` - Get promotion analytics
- `PUT /api/v1/spotlight/:id/targeting` - Update targeting
- `DELETE /api/v1/spotlight/:id` - Cancel promotion

### AI Tools
- `POST /api/v1/ai-matching` - Get AI match scores
- `GET /api/v1/matching` - Rule-based matching
- `POST /api/v1/pitch-writer` - Generate application pitch
- `GET /api/v1/content-ideas` - Get content suggestions
- `GET /api/v1/best-time` - Optimal posting times
- `GET /api/v1/market-rates` - Rate benchmarking
- `GET /api/v1/revenue-optimization` - Earning tips
- `GET /api/v1/rising-stars` - Fast-growing creator detection

## Best Practices

### Marketplace Browsing
✅ **DO:**
- Check marketplace daily for new campaigns
- Use AI match scores to prioritize (80%+ = great fit)
- Read full campaign details before applying
- Save campaigns to compare side-by-side
- Apply within 24 hours of campaign posting (early applicants get priority)
- Use filters to find perfect-fit campaigns

❌ **DON'T:**
- Apply to every campaign (quality over quantity)
- Ignore AI match scores (they're accurate predictors)
- Skip reading requirements (leads to rejection)
- Wait too long to apply (spots fill fast)
- Apply outside your niche (low success rate)

### Direct Offers
✅ **DO:**
- Respond within 24 hours (brands value responsiveness)
- View offers immediately (shows you're active)
- Negotiate professionally if budget is low
- Ask clarifying questions before accepting
- Check brand's profile and past creator reviews

❌ **DON'T:**
- Let offers expire (7-day limit)
- Accept every offer without evaluation
- Ignore low offers (politely decline or counter)
- Ghost brands mid-negotiation
- Accept without reading full terms

### Spotlight Usage
✅ **DO:**
- Use Spotlight when launching new profile
- Target specific industries matching your niche
- Track ROI (inquiries / cost)
- Run 14-30 day promotions for best results
- Update portfolio before activating Spotlight

❌ **DON'T:**
- Use Spotlight with incomplete profile
- Target too broadly (wastes impressions)
- Run Spotlight without competitive rates
- Forget to monitor analytics
- Activate during slow seasons
