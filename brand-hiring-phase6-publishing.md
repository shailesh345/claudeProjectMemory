# Phase 6: Publishing & Go-Live

## Status Flow
```
Approved - Ready to Publish → Live - Tracking → (continues to Phase 7)
```

## Content Published

### Publishing Flow

**Creator Action:**
1. Content approved by brand
2. Creator schedules post on platform (Instagram/TikTok/YouTube)
3. Content goes live at scheduled time
4. Creator returns to dashboard
5. Marks campaign as "Published" with live post link

**Automatic Detection:**
- Platform detects post via API integration (if connected)
  - Instagram Graph API
  - YouTube Data API
  - TikTok Business API
- Tracking begins immediately
- Brand receives notification

### Brand Notification
- Email: "Content is now live!"
- In-app: "Creator posted content - now tracking performance"
- Push notification with link to post

### Brand Verification

**Route**: `/app/tools/campaigns/:id/live`

**Verification Checklist:**
- ✅ **Post is live**: Click through to verify on platform
- ✅ **Correct hashtags**: All required hashtags present
- ✅ **Correct @mentions**: Brand handles mentioned
- ✅ **Disclosure present**: #ad, #sponsored, etc. visible
- ✅ **Link in bio**: If required, link is active (click test)
- ✅ **Posted at agreed time**: Timestamp matches schedule (+/- 30 min tolerance)
- ✅ **Content matches approved version**: No unauthorized changes

**If Issues Found:**
- Minor issues: Message creator to fix
- Major issues: May request take-down and repost
- Compliance issues (missing #ad): Immediate message + FTC risk

### Status Update
- **Status**: `Live - Tracking`
- **Live Since**: Timestamp
- **Post URL**: Clickable link to content
- **Platform**: Instagram/YouTube/TikTok icon

## Campaign Tracking & Monitoring

### Analytics Dashboard Route
**Route**: `/app/tools/campaigns/:id/analytics`

### Real-Time Metrics Display

**Hero Metrics (4 Large Cards):**
1. **👁️ Views/Impressions**: Total reach count
2. **❤️ Likes**: Like count with trend arrow
3. **💬 Comments**: Comment count with sentiment indicator
4. **🔄 Shares**: Share/repost count

**Secondary Metrics (Smaller Cards):**
5. **💾 Saves**: Bookmark/save count
6. **📊 Engagement Rate**: Calculated % (likes + comments + shares / reach)
7. **🔗 Link Clicks**: UTM tracked link clicks
8. **🛒 Conversions**: E-commerce sales (if integrated)

### Performance Graphs

**Time-Series Charts:**
- **Views Over Time**: Hourly for first 24h, daily after
- **Engagement Timeline**: Shows when engagement peaked
- **Growth Rate**: Velocity of metric changes

**Comparison Charts:**
- **This Campaign vs. Average**: How it compares to similar campaigns
- **Expected vs. Actual**: Projected reach vs. real reach
- **Platform Comparison**: If multi-platform, show side-by-side

### Detailed Analytics

**Audience Demographics (if available via API):**
- **Age Groups**: Pie chart (18-24, 25-34, 35-44, 45+, 55+)
- **Gender Split**: Bar chart (Male, Female, Other %)
- **Top Locations**: Map or list (cities, countries)
- **Device Type**: Mobile vs. Desktop
- **New vs. Existing Followers**: Reach beyond creator's audience

**Engagement Breakdown:**
- **Comment Sentiment Analysis**: Positive (green), Neutral (gray), Negative (red) %
- **Top Comments**: Most liked comments (shows brand perception)
- **Share Locations**: Where content was shared (DMs, Stories, Feeds)
- **Save Rate**: High save rate = valuable content

**Traffic & Conversions:**
- **UTM Tracking**: Clicks by source, medium, campaign
- **Link Click Map**: Geographic distribution of clicks
- **Conversion Funnel**: Click → Landing Page → Add to Cart → Purchase
- **Revenue Attributed**: Sales directly from campaign (requires e-commerce integration)

**Competitor Benchmarking (if data available):**
- How this campaign compares to competitor's influencer campaigns
- Industry averages for similar content
- Creator's performance vs. their average

### ROI Calculator

**Cost Inputs:**
- Creator payment: $X
- Platform fee: $Y
- Production costs: $Z (if any)
- **Total Cost**: $Total

**Value Outputs:**
- **Cost per Impression**: Total cost / impressions
- **Cost per Engagement**: Total cost / engagements
- **Cost per Click**: Total cost / clicks
- **Cost per Conversion**: Total cost / conversions
- **Revenue Generated**: Sales attributed to campaign
- **ROI %**: (Revenue - Cost) / Cost × 100
- **ROAS**: Revenue / Cost (Return on Ad Spend)

### Export & Sharing Options

**Export Formats:**
- **PDF Report**: Formatted, printable, with charts
- **CSV Data**: Raw metrics for analysis in Excel
- **PowerPoint**: Presentation-ready slides with charts
- **Google Sheets**: Live-updating sheet (via integration)

**Share Options:**
- **Shareable Link**: Public or password-protected link
- **Email Report**: Send to stakeholders
- **Slack Integration**: Post updates to Slack channel
- **Dashboard Embed**: Embed analytics in external dashboard

## Database Models (Inferred)
- `CampaignPost` (published content)
  - campaignId
  - submissionId
  - postedAt
  - platformPostId (external ID)
  - postUrl
  - verified (boolean)
  - verifiedAt
  - verificationNotes

- `CampaignMetrics` (time-series data)
  - postId
  - recordedAt (timestamp)
  - views, likes, comments, shares, saves
  - engagementRate
  - linkClicks
  - conversions
  - revenue

- `CampaignAudience` (demographics)
  - postId
  - ageGroups (JSON)
  - genderSplit (JSON)
  - topLocations (JSON)
  - deviceBreakdown (JSON)

- `CampaignComment` (comment analysis)
  - postId
  - commentId (external)
  - text
  - sentiment (positive/neutral/negative)
  - likes
  - postedAt

- `TrafficSource` (UTM tracking)
  - postId
  - source, medium, campaign
  - clicks
  - conversions
  - revenue

## API Endpoints (Inferred)
- `POST /api/campaigns/:id/mark-published` - Creator marks as published
- `GET /api/campaigns/:id/verify` - Get verification checklist
- `POST /api/campaigns/:id/verify` - Submit verification
- `GET /api/campaigns/:id/metrics` - Get current metrics
- `GET /api/campaigns/:id/metrics/history` - Time-series data
- `GET /api/campaigns/:id/audience` - Audience demographics
- `GET /api/campaigns/:id/comments` - Comment analysis
- `GET /api/campaigns/:id/traffic` - UTM tracking data
- `GET /api/campaigns/:id/roi` - ROI calculation
- `POST /api/campaigns/:id/export` - Generate export (PDF/CSV/PPTX)
- `POST /api/campaigns/:id/share` - Create shareable link

### Social Media API Integrations
- `GET /external/instagram/:postId/insights` - Instagram metrics
- `GET /external/youtube/:videoId/analytics` - YouTube analytics
- `GET /external/tiktok/:videoId/stats` - TikTok data

## Tracking Implementation

### UTM Parameters
All links include tracking parameters:
```
https://brand.com/product?utm_source=instagram&utm_medium=influencer&utm_campaign=spring_launch_2024&utm_content=creator_username
```

### Tracking Pixels
- Embed tracking pixel in landing pages
- Fire pixel on: page view, add to cart, purchase
- Send data back to platform for attribution

### API Polling Schedule
- **First 24 hours**: Poll every 15 minutes
- **Days 2-7**: Poll hourly
- **After 7 days**: Poll daily
- **After 30 days**: Final snapshot, then stop

## Notifications During Live Period

### To Brand
- "Your campaign just reached 100K views!" (milestone alerts)
- "Engagement rate is 5.2% - above average!" (performance alerts)
- "Someone left a question in comments" (engagement opportunities)
- Daily summary: "Your campaign today: +25K views, +1.2K likes"

### To Creator
- "Your content is performing great! 4.8% engagement so far"
- "Brand might want to see this comment - looks important"

## Best Practices

✅ **DO:**
- Verify post immediately after going live
- Monitor closely in first 24-48 hours (peak performance)
- Respond to comments (brand or encourage creator to)
- Celebrate milestones with creator (boosts morale)
- Track metrics daily during campaign
- Look for unexpected audience segments (insights for future)

❌ **DON'T:**
- Assume everything is fine without verification
- Ignore negative comments (address them quickly)
- Compare metrics too early (give it 24-48 hours)
- Obsess over minute-by-minute changes
- Forget to attribute conversions properly

## Edge Cases

### Post Doesn't Go Live
- Creator marks as published but no API detection
- Manual verification required
- May need creator to provide screenshots or proof

### Post Removed/Deleted
- Platform detects post deletion
- Status → `Post Removed - Investigation`
- Notify brand immediately
- Investigate reason (creator error, platform violation, etc.)

### Poor Performance
- If metrics significantly below expected:
  - Platform suggests content boost (paid promotion)
  - Analyze why (timing, audience mismatch, content quality)
  - Use insights for future campaigns

### Viral Performance
- If metrics significantly exceed expected:
  - Celebrate with creator
  - Consider extending tracking period
  - Negotiate usage rights extension
  - Plan follow-up campaign
