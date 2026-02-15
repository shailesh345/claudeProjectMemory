# Project Memory - 1THDB Influencer Platform

## Architecture
- **Stack**: React 18 + Vite frontend, Express.js backend, PostgreSQL + Prisma ORM
- **Frontend entry**: `src/App.jsx`, pages in `src/web/pages/`
- **Backend entry**: `backend/server.js`, 77 controllers, 97 routes, 113 services
- **Database schema**: `database/prisma/schema.prisma` (168+ models, ~4500 lines)
- **State management**: 31+ Zustand stores in `src/store/`
- **Auth**: JWT tokens via `backend/services/tokenService.js`, 2FA support

## Dev Server
- **Always use `node start-dev.js`** to run the full app (frontend + backend + Prisma Studio)
- Frontend: localhost:3000 (Vite), Backend: localhost:3001 (Express), Prisma Studio: localhost:5555
- Script kills existing processes on ports 3000/3001/5555 before starting

## Key Patterns
- Prisma uses raw SQL in some controllers (`$queryRaw` tagged templates are safe; `$queryRawUnsafe` needs parameterized $1/$2 placeholders)
- Migration files are loose .sql files in `database/prisma/migrations/` (not standard Prisma timestamped folders)
- Audit logging via `backend/services/auditLogService.js` (logSecurityAction, logPaymentAction, etc.)
- Frontend logger utility at `src/atomic/utils/logger.js` (env-aware, silences in production)
- Response helper at `backend/utils/response.js` (success/error/paginated)

## Security Audit (Feb 2026)
- Fixed SQL injection in brandCampaignController, savedCreatorsController (whitelist + parameterization)
- Fixed CORS: exact origin matching only (no prefix match)
- Debug telemetry: explicit opt-in via DEBUG_TELEMETRY=true env var
- AI AutoFix /log endpoint: now requires auth + rate limiting
- Auth rate limiting: 5 attempts/15min in production
- XSS sanitization: multi-pass with HTML entity decoding
- JWT: hard fail if JWT_SECRET not set in production
- Captcha test mode: gated behind import.meta.env.DEV
- Demo accounts fallback: gated behind import.meta.env.DEV

## Brand → Influencer Hiring Flow (8 Phases)

### Phase 1: Discovery (Routes: `/app/tools/discovery`, `/app/explore`, `/app/search`)
- Advanced filters: niche, followers, platform, location, engagement, price
- AI-powered matching with compatibility scores (toggle "AI Mode")
- Grid/list view modes; actions: save, view profile, message, compare
- Creator profile view at `/app/profile/:username` shows: trust signals (verified, top rated, campaigns completed), metrics (reach, engagement, views, success rate), tabs (overview, portfolio, audience, pricing)

### Phase 2: Initial Contact (Route: `/app/messages`)
- Message templates: "Collaboration Inquiry", "Campaign Brief", "Quick Partnership Pitch"
- Features: file attachments, real-time chat, read receipts, video calls
- "Download Media Kit" button on profile → PDF with bio, stats, demographics, rate card, testimonials

### Phase 3: Campaign Creation (Route: `/app/tools/campaigns`)
- **8-step form**: Basic info (name, type, goal) → Creator selection → Deliverables (platform, content types, requirements) → Guidelines (assets, messaging, approval process) → Timeline (start, submission, publish, end dates) → Budget & payment (terms, usage rights, exclusivity) → Legal (contracts, NDA, FTC) → Review & submit
- Campaign types: Sponsored Post, Product Review, Brand Ambassador, Event Coverage, Unboxing, Tutorial, Giveaway
- Deliverables: posts, stories, reels, videos, IGTV (with quantities)
- Payment terms: upfront (50/50), upon completion, net 30/60/90, milestone-based
- Usage rights: duration (30/90 days, 1 year, perpetual), platforms, exclusivity clause

### Phase 4: Negotiation (Status transitions)
- Creator receives email + in-app notification → Actions: Accept, Decline, Negotiate, Ask questions
- **Status flow**: `Pending Acceptance` → `In Negotiation` → `Active - Awaiting Content`
- Negotiation points: budget, deliverables, timeline, usage rights, payment terms
- Auto-actions on accept: contract generation, e-signatures, payment hold (escrow), dashboard activation

### Phase 5: Content Creation (Route: `/app/tools/campaigns/:id`)
- **Status flow**: `In Progress` → `Under Review` → `Approved - Ready to Publish` / `Revision Requested` / `Rejected`
- Content review at `/app/tools/campaigns/:id/review` shows: preview, platform, caption, hashtags, mentions, links, posting time
- Brand actions: Approve (with optional comment), Request Changes (with detailed feedback), Reject (rare, requires explanation)
- Revision process: typically 2-3 rounds max

### Phase 6: Publishing (Route: `/app/tools/campaigns/:id/analytics`)
- **Status**: `Live - Tracking`
- Real-time metrics: views, likes, comments, shares, saves, engagement rate, link clicks (UTM), conversions
- Brand verifies: correct hashtags, @mentions, disclosure (#ad, #sponsored), link in bio, posting time
- Analytics dashboard: performance graphs, best content, demographics, sentiment analysis, competitor benchmarking, ROI

### Phase 7: Payment (Route: `/app/tools/billing`)
- **Status flow**: `Completed - Pending Payment` → `Completed - Paid`
- Platform payment: escrow hold → review performance → approve payment → funds released (platform fee ~10%)
- Payment methods: card, bank transfer, PayPal, wire, crypto
- Review & rating: 5-star ratings (overall, communication, content quality, professionalism, timeliness) + written review

### Phase 8: Post-Campaign (Route: `/app/tools/campaigns/:id/report`)
- Comprehensive report: performance summary, goal comparison, audience insights, content performance, financial summary (cost per engagement/click/conversion, ROI), recommendations
- Export options: PDF, PowerPoint, CSV, shareable link
- Actions: add to "Preferred Partners", set alerts, send follow-up for next campaign
- Content repurposing at `/app/tools/campaigns/:id/assets`: download originals, track usage rights

### Alternative Flows
- **Brand Ambassador**: long-term retainer campaigns with monthly deliverables, recurring payment
- **Multi-Creator Campaigns**: 3-10 creators, same brief, staggered posting, comparative analysis
- **Rush Campaigns**: "Express" flag, higher budget, 24-48hr turnaround, priority support
- **Product Seeding**: $0 budget, no guaranteed post, track shipment, monitor organic posts

### Key Integrations
- Payment: Stripe, PayPal, escrow
- Social APIs: Instagram Graph, YouTube Analytics, TikTok Business, Twitter
- E-commerce: Shopify, WooCommerce (conversion tracking)
- Analytics: Google Analytics (UTM), custom pixels
- CRM: Salesforce, HubSpot

### Detailed Phase Documentation (Brand → Influencer)
See memory directory for in-depth phase documentation:
- `brand-hiring-phase1-discovery.md` - Discovery, filters, profiles, AI matching
- `brand-hiring-phase2-contact.md` - Messaging, media kits, templates
- `brand-hiring-phase3-campaign-creation.md` - 8-step form, deliverables, legal
- `brand-hiring-phase4-negotiation.md` - Counter-offers, contracts, escrow
- `brand-hiring-phase5-content-creation.md` - Submission, review, revisions
- `brand-hiring-phase6-publishing.md` - Go-live, tracking, analytics, ROI
- `brand-hiring-phase7-payment.md` - Escrow release, disputes, reviews
- `brand-hiring-phase8-post-campaign.md` - Reports, content rights, partnerships

## Influencer → Brand Approach Flow (10 Phases)

### Phase 0: Preparation (Routes: `/app/tools/upgrade`, `/app/tools/media-kit`, `/app/tools/creator`)
- **Upgrade to Influencer**: 4-step wizard, eligibility check (1K followers, 2% engagement, 10 content), OAuth connect (9 platforms), 15 permissions unlocked
- **Influencer Tiers**: Micro (1K-10K, $50-$500), Mid (10K-100K, $500-$5K), Macro (100K-1M, $5K-$25K), Mega (1M+, $25K-$1M+)
- **Media Kit Builder**: Bio, tagline, categories, rate card (per platform/content type), portfolio, achievements, public URL (`/media-kit/:slug`)
- **Influencer Score**: 8 composite scores (overall, engagement, reach, growth, consistency, brand safety, audience quality, authenticity), per-platform scores (YouTube, Instagram, Facebook, TikTok)
- **KYC Verification**: Required for withdrawals, government ID, personal info verification

### Phase 1: Discovery (Routes: `/app/marketplace`, `/app/direct-offers`, `/app/tools/spotlight`)
- **Campaign Marketplace**: 4 tabs (For You AI-recommended, Trending, Highest Pay, Closing Soon), filters (platform, budget, niche, content type, duration, location), AI match scores
- **Direct Offers**: Brand-initiated offers, 5 tabs (Pending, Negotiating, Accepted, Rejected, Expired), 7-day expiry, status lifecycle
- **Spotlight Promotion**: Paid visibility boost (Basic/Standard/Premium tiers), targeting (industry, company size, budget, location), analytics tracking
- **AI Tools**: AI Studio, Content Ideas, Best Time to Post, Pitch Writer, Revenue Optimization, Market Rates benchmarking

### Phase 2: Application (Routes: `/app/marketplace`, `/app/direct-offers`, `/app/tools/collaborations`)
- **Campaign Application**: Quick Apply, personal message (150-500 chars), proposed rate, status (PENDING/APPROVED/REJECTED/WITHDRAWN)
- **Direct Offer Response**: 3 options (Accept → contract, Counter-Offer → negotiation, Reject), status lifecycle, 7-day expiry
- **Collaboration Proposals**: Via `/api/v1/collaborations`, less formal than contracts, flexible terms

### Phase 3: Negotiation (Routes: `/app/direct-offers`, `/app/messages`)
- **Counter-Offer Exchange**: Round-based negotiation, negotiationRound counter, 10 key points (budget, deliverables, revisions, usage rights, duration, exclusivity, timeline, product seeding, partnerships)
- **Messaging**: Real-time (Socket.io), read receipts, typing indicators, file attachments, GIFs (Giphy), voice/video calls
- **Rate Card Baseline**: Published rates as starting point, `/api/v1/market-rates` for benchmarking

### Phase 4: Contract (Routes: `/app/contracts/:contractId`, `/app/escrow/:campaignId`)
- **Contract Review**: 6 sections (deliverables, financial terms, timeline, guidelines, required elements, rights & exclusivity)
- **E-Signature**: Digital signing, records (creatorSignedAt, creatorSignatureIP, brandSignedAt, brandSignatureIP), both required for activation
- **Escrow Protection**: 10% platform fee, milestone-based releases, status lifecycle (PENDING → FUNDED → PARTIALLY_RELEASED → RELEASED), dispute resolution

### Phase 5: Content Creation (Routes: `/app/create-post`, `/app/create/short`, `/app/tools/ai-studio`)
- **Brief Review**: Pre-production checklist (hashtags, mentions, guidelines, assets, timeline)
- **AI Tools**: AI Studio (caption/script generation), Content Ideas API, Best Time to Post API
- **Creation Tools**: Post/short/vlog creation, video editor (`/api/v1/video-editor`), live streaming, drafts system (`/api/v1/drafts`), content queue (`/api/v1/queue`)
- **Communication**: Progress updates, clarifying questions, timeline concerns via `/app/messages`

### Phase 6-8: Submission, Publishing, Payment (combined phase file)
- **Phase 6 Submission**: Submit via `/api/v1/content-submissions`, 3 outcomes (Approved, Revision Requested, Rejected), revision lifecycle
- **Phase 7 Publishing**: Publishing checklist (hashtags, mentions, FTC disclosure), cross-platform options, performance tracking (views, likes, comments, shares, saves, engagement rate, link clicks, conversions)
- **Phase 8 Payment**: Escrow milestone releases, 10% platform fee deduction, wallet management (`/app/wallet`), withdrawals (Stripe Connect, 2-5 days), invoice management

### Phase 9: Growth (Routes: `/app/tools/media-kit`, `/app/tools/creator`, `/app/tools/monetization`)
- **Brand Reviews**: 5-star ratings (overall, communication, content quality, professionalism, timeliness), impacts avgCampaignRating, Top Rated badge (4.8+ with 5+ reviews)
- **Portfolio Updates**: Add completed campaigns, update rate card (10-20% increase per 2x follower growth), refresh achievements
- **Performance Analysis**: Dashboard components (GrowthChart, ContentPerformance, AudienceDemographics, CampaignHistory, PlatformComparison)
- **Long-term Relationships**: Thank-you messages, share results, express future interest, follow brand socially
- **Revenue Diversification**: 10 streams (brand deals 50-60%, affiliate 15-20%, subscriptions 10-15%, digital products 10-15%, other 5-10%)

### Alternative Flows
- **UGC Creation**: Brand uses content for ads, creator doesn't post, raw files delivered, higher rates for ad usage
- **Product Seeding**: Free products, no posting obligation, optional honest review, leads to paid campaigns
- **Ambassador Programs**: 3-12 month commitment, recurring monthly deliverables/payment, exclusive representation
- **Multi-Brand Management**: Track exclusivity clauses, workload management, content differentiation
- **Rush Campaigns**: 25-50% premium for 24-48hr turnaround, same-day = 50-100% premium
- **Dispute Resolution**: Via `/api/v1/disputes`, evidence submission, platform mediation, escrow protection

### Detailed Phase Documentation (Influencer → Brand)
- `influencer-phase0-preparation.md` - Upgrade, media kit, influencer score, KYC
- `influencer-phase1-discovery.md` - Marketplace, direct offers, spotlight, AI tools
- `influencer-phase2-application.md` - Campaign applications, offer responses, collaborations
- `influencer-phase3-negotiation.md` - Counter-offers, messaging, rate card benchmarking
- `influencer-phase4-contract.md` - Contract review, e-signature, escrow protection
- `influencer-phase5-content-creation.md` - Brief review, AI tools, creation, communication
- `influencer-phase6-7-8.md` - Submission, publishing, payment (combined)
- `influencer-phase9-growth.md` - Reviews, portfolio, analysis, relationships, diversification
