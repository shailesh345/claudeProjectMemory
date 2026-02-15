# Phase 0: Preparation & Profile Building

## 0.1 Become an Influencer

### Route
**Primary**: `/app/tools/upgrade`

### 4-Step Upgrade Wizard

#### Step 1: Review Eligibility

**Global Requirements:**
- Minimum 1,000 followers
- Minimum 2.0% engagement rate
- Minimum 10 content pieces
- Account age >= 30 days
- Verified email AND phone
- No strikes in last 90 days

**Platform-Specific Requirements (meet at least 1):**

| Platform | Min Followers | Min Content |
|----------|---------------|-------------|
| YouTube | 1,000 subscribers | 10 videos |
| Instagram | 1,000 followers | 20 posts |
| TikTok | 1,000 followers | 15 videos |
| Twitter | 500 followers | 100 tweets |
| Facebook | 500 followers | 10 posts |
| VK | 500 followers | 10 posts |

#### Step 2: Connect Social Accounts
- Link social media accounts via OAuth
- **Supported Platforms**: YouTube, Instagram, TikTok, Facebook, Twitter, VK, Behance (9 OAuth providers total)
- **Route**: `/app/settings` → Social Media tab
- **Backend**: `/api/v1/social-media`, `/api/v1/social-media-data`

#### Step 3: Verify Metrics
- Platform auto-fetches follower count, engagement rate, audience data
- Review pulled metrics for accuracy
- Metrics update periodically via social media APIs

#### Step 4: Submit for Review
- Application reviewed (automated + manual if needed)
- Upon approval:
  - `isInfluencer` flag set to `true`
  - `influencerSince` timestamp recorded
  - `influencerTier` assigned based on follower count
  - All 15 influencer permissions granted

### Permissions Unlocked (15 total)
- `canLiveStream` - Access live streaming studio
- `canMonetize` - Enable monetization features
- `canJoinCampaigns` - Apply to brand campaigns
- `canAccessInfluencerDashboard` - Creator dashboard access
- `canManageInfluencerProfile` - Edit influencer-specific profile fields
- `canAccessMonetization` - Monetization dashboard
- `canViewEarnings` - View earnings analytics
- `canWithdrawFunds` - Withdraw from wallet
- `canCreateMerchandise` - Create merchandise
- `canHostPaidEvents` - Host paid events
- `canCreateCommunities` - Create communities
- `canAccessAnalytics` - Analytics dashboard
- `canViewReports` - View performance reports
- `canUploadVRContent` - Upload VR/360 content
- `canUpload3DContent` - Upload 3D content

### Influencer Tiers (auto-assigned by follower count)

| Tier | Followers | Typical Rate (per post) |
|------|-----------|------------------------|
| **MICRO** | 1K - 10K | $50 - $500 |
| **MID** | 10K - 100K | $500 - $5K |
| **MACRO** | 100K - 1M | $5K - $25K |
| **MEGA** | 1M+ | $25K - $1M+ |

### Status After Approval
**Status**: `Influencer Account Active`

## 0.2 Build Your Media Kit

### Route
**Primary**: `/app/tools/media-kit`

### Media Kit Sections

#### Profile Information
- **Bio**: Professional biography (textarea)
- **Tagline**: One-line description of your brand
- **Categories/Niches**: Select content niches
  - Fashion, Tech, Beauty, Lifestyle, Gaming, Fitness, Food, Travel, Parenting, etc.
- **Profile Image**: Professional headshot

#### Rate Card
**Per Platform:**
- Instagram rates
- YouTube rates
- TikTok rates
- Twitter rates

**Per Content Type:**
- Feed Post
- Reel/Short
- Story
- Dedicated Video
- Integration/Mention
- Live Stream

**Custom Quote Option**: Message button for non-standard packages

#### Portfolio
- Upload past work samples
- Thumbnails with engagement metrics
- Categorized by content type and platform
- Shows brand partnerships and results

#### Achievements & Awards
- Platform milestones (100K followers, 1M views, etc.)
- Industry recognition
- Certifications
- Awards won

#### Past Brand Partnerships
- Brand names and logos
- Campaign type and results
- Testimonials from brands
- Performance metrics

### Actions Available
- 💾 **Save**: Save changes
- 👁️ **Preview**: See how brands will view your kit
- 🔗 **Share**: Get public shareable URL (`/media-kit/:slug`)
- 📄 **Download PDF**: Export as PDF document
- 📋 **Copy URL**: Copy public link to clipboard

### Deliverable
Professional media kit with rate card accessible via public URL

## 0.3 Optimize Your Influencer Score

### Route
**Primary**: `/app/tools/creator`

### Scoring Components (stored in `influencer_scores` table)

**Overall Composite Scores:**
- **Overall Score** - Weighted composite of all factors
- **Engagement Score** - How well your audience interacts
- **Reach Score** - Total audience size across platforms
- **Growth Score** - Follower growth velocity
- **Consistency Score** - Posting frequency and regularity
- **Brand Safety Score** - Content safety assessment
- **Audience Quality** - Real vs fake follower ratio
- **Audience Authenticity** - Genuine engagement patterns

**Per-Platform Scores:**
- YouTube score
- Instagram score
- Facebook score
- TikTok score

### Key Metrics Brands See
- ⭐ **Average campaign rating** (`avgCampaignRating`) - 0-5 stars
- ⏱️ **Average response time** (`avgResponseTimeHours`) - Hours to respond
- ✅ **Campaign completion rate** (`campaignCompletionRate`) - % campaigns completed
- 📅 **On-time delivery rate** (`onTimeDeliveryRate`) - % delivered on time
- 🔒 **Brand safety score** (`brandSafetyScore`) - 0-100
- 🎯 **Professional score** (`professionalScore`) - 0-100
- 👥 **Real followers percentage** (`realFollowersPercent`) - % authentic followers

### Tips to Improve Score
- Respond to messages within 24 hours
- Complete every campaign on time
- Earn 4.5+ star ratings consistently
- Maintain authentic audience growth (no fake followers)
- Post regularly across platforms
- Keep content brand-safe (avoid controversial topics)
- High engagement rate (not just follower count)

## 0.4 Complete KYC Verification

### Route
**Backend**: `/api/v1/kyc`

### Process
1. Submit government-issued ID (passport, driver's license)
2. Verify personal information (name, DOB, address)
3. Complete verification for payment eligibility
4. Required before first withdrawal

### Status After Verification
**Status**: `Verified Creator`

### Why KYC is Required
- Legal requirement for payment processing
- Anti-fraud protection
- Tax compliance (1099 forms if US-based)
- Enables Stripe Connect for withdrawals

## Database Models (Inferred)

### User Model (extended for influencers)
```
User {
  isInfluencer: boolean
  influencerSince: timestamp
  influencerTier: enum (MICRO, MID, MACRO, MEGA)
  stripeConnectId: string (for payouts)
  permissions: array (15 influencer permissions)
}
```

### MediaKit Model
```
MediaKit {
  userId: foreign key
  bio: text
  tagline: string
  categories: array (niches)
  rateCard: JSON {
    instagram: { post, reel, story, video, mention, liveStream }
    youtube: { ... }
    tiktok: { ... }
    twitter: { ... }
  }
  portfolio: array of assets
  achievements: array
  brandPartnerships: array
  publicSlug: string (unique URL slug)
  updatedAt: timestamp
}
```

### InfluencerScore Model (table: `influencer_scores`)
```
InfluencerScore {
  userId: foreign key
  overallScore: float
  engagementScore: float
  reachScore: float
  growthScore: float
  consistencyScore: float
  brandSafetyScore: float
  audienceQuality: float
  audienceAuthenticity: float
  avgCampaignRating: float
  avgResponseTimeHours: float
  campaignCompletionRate: float
  onTimeDeliveryRate: float
  professionalScore: float
  realFollowersPercent: float
  youtubeScore: float
  instagramScore: float
  facebookScore: float
  tiktokScore: float
  lastCalculated: timestamp
}
```

### SocialMediaConnection Model
```
SocialMediaConnection {
  userId: foreign key
  platform: enum (YOUTUBE, INSTAGRAM, TIKTOK, FACEBOOK, TWITTER, VK, BEHANCE, etc.)
  platformUserId: string
  platformUsername: string
  accessToken: encrypted string
  refreshToken: encrypted string
  followers: integer
  engagementRate: float
  lastSync: timestamp
  connected: boolean
}
```

### KYC Model
```
KYC {
  userId: foreign key
  status: enum (PENDING, APPROVED, REJECTED)
  idType: enum (PASSPORT, DRIVERS_LICENSE, NATIONAL_ID)
  idNumber: encrypted string
  idFileUrl: encrypted string
  fullName: string
  dateOfBirth: date
  address: string
  verifiedAt: timestamp
  verifiedBy: admin ID (nullable)
}
```

## API Endpoints (Inferred)

### Upgrade to Influencer
- `POST /api/v1/upgrade/check-eligibility` - Check if user meets requirements
- `POST /api/v1/upgrade/submit` - Submit upgrade application
- `GET /api/v1/upgrade/status` - Check application status

### Social Media
- `GET /api/v1/social-media` - Get connected accounts
- `POST /api/v1/social-media/connect` - Connect new platform (OAuth)
- `DELETE /api/v1/social-media/:platform` - Disconnect platform
- `POST /api/v1/social-media/sync` - Manually sync metrics
- `GET /api/v1/social-media-data` - Get aggregated social data

### Media Kit
- `GET /api/v1/media-kit` - Get own media kit
- `GET /api/v1/media-kit/:slug` - Get public media kit by slug
- `PUT /api/v1/media-kit` - Update media kit
- `POST /api/v1/media-kit/generate-pdf` - Generate PDF download
- `GET /api/v1/media-kit/preview` - Preview how brands see your kit

### Influencer Score
- `GET /api/v1/influencer-score` - Get current scores
- `POST /api/v1/influencer-score/recalculate` - Request score recalculation
- `GET /api/v1/influencer-score/history` - Score history over time

### KYC
- `POST /api/v1/kyc/submit` - Submit KYC documents
- `GET /api/v1/kyc/status` - Check verification status
- `POST /api/v1/kyc/resend` - Resubmit if rejected

## Best Practices

### Profile Completion
✅ **DO:**
- Complete 100% of media kit sections
- Use professional, high-quality photos
- Set competitive rates (benchmark with `/api/v1/market-rates`)
- Upload 5+ diverse portfolio samples
- Keep bio concise but compelling (150-300 words)
- Update rate card as you grow
- Connect all active social accounts

❌ **DON'T:**
- Leave rate card empty (brands skip profiles without pricing)
- Use outdated or low-quality portfolio samples
- Inflate metrics or use fake followers
- Skip categories/niches (hurts discoverability)
- Forget to update after hitting follower milestones
- Use unprofessional or casual profile photos

### Score Optimization
✅ **DO:**
- Respond to all messages within 24 hours
- Accept realistic campaigns you can deliver
- Complete every campaign on time (95%+ completion rate)
- Request reviews from satisfied brands
- Post consistently (3-5x per week minimum)
- Maintain authentic growth (organic followers)

❌ **DON'T:**
- Buy fake followers or engagement
- Accept more campaigns than you can handle
- Miss deadlines
- Ghost brands mid-conversation
- Post sporadically (hurts consistency score)
- Engage in controversial or unsafe content
