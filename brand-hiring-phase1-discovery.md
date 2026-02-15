# Phase 1: Discovery & Research

## Routes
- **Primary**: `/app/tools/discovery` - Creator Discovery Page
- **Secondary**: `/app/explore` - General exploration
- **Search**: `/app/search` - Search by keywords, niche, platform

## Features & Filters

### Advanced Filters
- **Niche/Category**: Fashion, Tech, Beauty, Lifestyle, etc.
- **Follower Range**: 100K-500K, 500K-1M, 1M-5M, 5M+
- **Platform**: Instagram, YouTube, TikTok, Twitter
- **Location**: Geographic filtering
- **Engagement Rate**: Percentage-based filtering
- **Price Range**: Budget-based filtering

### AI-Powered Matching
- Toggle "AI Mode" for smart recommendations
- Analyzes brand profile and past campaigns
- Returns ranked matches with compatibility scores
- Uses brand history for better suggestions

### View Modes
- **Grid View**: Quick browsing with cards
- **List View**: Detailed comparison with more data

## Actions Available
- ❤️ **Save to Favorites**: Add creator to saved list
- 👁️ **View Profile**: Navigate to `/app/profile/:username`
- 💬 **Quick Message**: Send direct message
- 🔍 **Compare**: Multi-creator comparison tool

## Creator Profile View (`/app/profile/:username`)

### Hero Section
**Trust Signals:**
- ✅ Verified badge
- ⭐ Top Rated badge (95%+ success rate)
- 🏆 Campaigns completed count
- ⭐ Average rating (X.X/5.0)
- ⏱️ Response time

**Key Metrics (4 Cards):**
1. Total Reach (followers across platforms)
2. Avg Engagement Rate (%)
3. Avg Views per post
4. Success Rate (%)

**Primary Actions:**
- 💼 **Hire Creator** (blue button) → Navigates to campaign creation
- 📄 **Download Media Kit** → PDF generation
- 💬 **Message** → Opens message thread

### Tab 1: Overview
- **Platform Performance**:
  - Breakdown by Instagram, YouTube, TikTok
  - Followers per platform
  - Engagement rate per platform
- **Brand Reviews**:
  - Past brand partners
  - Star ratings
  - Written testimonials
  - Campaign results

### Tab 2: Portfolio
- Past collaborations showcase
- Brand name
- Content type (Post, Story, Reel, Video)
- Thumbnail/image gallery
- Engagement metrics per piece
- View counts

### Tab 3: Audience
**Demographics:**
- Age groups (18-24, 25-34, 35-44, 45+)
- Gender split (Male/Female/Other %)
- Top locations (cities/countries)
- Audience interests (tags/categories)

### Tab 4: Pricing
**Package Tiers:**
- **Basic** (e.g., $500): 1 post, 1 story, 24h posting
- **Standard** (e.g., $1,200): 2 posts, 3 stories, 1 reel, 30-day usage rights ⭐ Most Popular
- **Premium** (e.g., $2,500): 3 posts, 5 stories, 2 reels, 90-day usage rights, content creation

**Custom Quote Option:**
- Message button for custom packages
- Negotiable pricing

## Decision Making Tools
- Compare multiple creators side-by-side
- Export comparison report
- Share profiles with team members

## Database Models (Inferred)
- `User` (role: CREATOR)
- `CreatorProfile` (bio, metrics, pricing)
- `Platform` (Instagram, YouTube, TikTok stats)
- `Portfolio` (past work)
- `Review` (brand ratings)
- `AudienceDemographics`
- `SavedCreator` (brand favorites)
- `PricingPackage`

## API Endpoints (Inferred)
- `GET /api/creators/discover` - Get filtered creator list
- `GET /api/creators/ai-match` - AI recommendations
- `GET /api/creators/:id/profile` - Full profile data
- `GET /api/creators/:id/portfolio` - Past work
- `GET /api/creators/:id/audience` - Demographics
- `GET /api/creators/:id/pricing` - Pricing packages
- `GET /api/creators/:id/media-kit` - Generate PDF
- `POST /api/creators/:id/save` - Save to favorites
- `GET /api/creators/compare` - Multi-creator comparison

## Best Practices
✅ **DO:**
- Define target audience before searching
- Use multiple filters to narrow results
- Save 10-15 creators to compare
- Check engagement rate, not just follower count
- Read reviews from other brands
- Look for authentic, engaged audiences

❌ **DON'T:**
- Choose based on follower count alone
- Ignore engagement metrics
- Skip reviewing portfolio
- Forget to check audience demographics
