# Phase 3: Negotiation & Terms

## 3.1 Counter-Offer Exchange

### Route
**Primary**: `/app/direct-offers` or `/app/tools/collaborations`

### Negotiation Workflow

**Step-by-Step Process:**

1. **You Send Counter-Offer**
   - Enter counter budget amount
   - Write detailed message explaining your reasoning
   - Submit counter-offer
   - `negotiationRound` increments (1 → 2 → 3...)

2. **Brand Reviews**
   - Brand receives notification of counter-offer
   - Brand sees side-by-side comparison:
     - Original offer (left)
     - Your counter-offer (right)
     - Differences highlighted

3. **Brand Responds** (3 options):

   **A. Accept Your Counter**
   - Agreement reached
   - Contract auto-generated
   - Move to Phase 4 (Contract & Commitment)
   - **Status**: `NEGOTIATING` → `ACCEPTED` → `CONVERTED_TO_CONTRACT`

   **B. Send New Counter-Offer**
   - Brand proposes different terms
   - You receive notification
   - You review and respond
   - Negotiation continues
   - **Status**: Stays `NEGOTIATING`

   **C. Reject**
   - Brand declines to continue negotiation
   - Offer ends
   - **Status**: `NEGOTIATING` → `REJECTED`

4. **Back-and-Forth**
   - Can go multiple rounds (typically 2-4 max)
   - Each party can see full negotiation history
   - All changes tracked and logged
   - Platform encourages compromise after 3 rounds

### Key Negotiation Points

**1. Base Compensation**
- Per deliverable pricing
- Total campaign budget
- Milestone-based payments
- Upfront percentage (50/50 vs 100% after)

**2. Number of Deliverables**
- Reduce quantity if budget is firm
- Add deliverables if budget increases
- Example: "I can do 2 posts + 3 stories for $1,500 instead of 3 posts"

**3. Revision Limits**
- How many revision rounds included (typically 2-3)
- Extra fee for additional revisions
- Example: "Includes 2 revision rounds, $100 per additional round"

**4. Content Usage Rights**
- How brand can reuse your content
- Platforms allowed (social only vs. social + website vs. all media including ads)
- Example: "Social media only" vs. "Social + website + paid ads"

**5. Usage Rights Duration**
- How long brand retains rights
- Options: 30 days, 90 days, 6 months, 1 year, perpetual
- Perpetual rights command 2-3x premium
- Example: "90-day usage rights" vs. "1-year usage rights for +$500"

**6. Exclusivity Period**
- Cannot promote competitors for X months
- Category-specific (e.g., can't promote other skincare brands)
- Example: "6-month exclusivity in beauty category"

**7. Exclusivity Premium**
- Additional compensation for exclusivity
- Typically 20-50% above base rate
- Example: "Base rate $1,000 + $300 exclusivity premium"

**8. Timeline Flexibility**
- Deadlines and posting schedule
- Rush premium (24-48 hour turnaround = 25-50% premium)
- Example: "Can deliver in 2 weeks for standard rate, or 1 week for +25%"

**9. Product Seeding**
- Free products included in deal
- Separate from monetary compensation
- Example: "$1,500 payment + $500 worth of products"

**10. Long-term Partnership**
- Discount for recurring collaboration
- Ambassador pricing (monthly retainer)
- Example: "One-time: $2,000 per campaign, Ambassador: $1,500/month for 3 campaigns"

## 3.2 Message-Based Discussion

### Route
**Primary**: `/app/messages`

### Messaging Features

**Real-Time Communication:**
- **Real-time messaging**: Powered by Socket.io
- **Read receipts**: See when brand read your message
- **Typing indicators**: See when brand is typing
- **Online status**: Green dot when brand is online

**Rich Media Support:**
- **File attachments**: Share examples, mood boards, references (max 25MB)
- **GIF support**: Giphy integration for casual communication
- **Link previews**: Automatic preview of shared URLs
- **Image previews**: Inline image display

**Advanced Features:**
- **Voice calls**: Audio call option for quick discussions
- **Video calls**: Face-to-face meetings for detailed planning
- **Group chat**: Multi-party conversations (brand team + you)
- **Message search**: Search conversation history
- **Starred messages**: Bookmark important messages

### Discussion Topics During Negotiation

**Creative Direction:**
- Content style preferences
- Visual aesthetic
- Tone (professional, casual, humorous, inspirational)
- References and examples

**Brand Messaging:**
- Key talking points to hit
- Messaging guidelines
- Do's and don'ts
- Brand voice and personality

**Specific Requirements:**
- **Hashtags**: Which hashtags to use
- **Mentions**: Brand @handles to tag
- **Disclosures**: #ad, #sponsored placement
- **Links**: URLs to include (UTM-tracked)

**Timeline Adjustments:**
- Flexibility on deadlines
- Optimal posting times
- Staggered posting (if multi-platform)

**Additional Assets:**
- Brand logos needed
- Product images
- Guidelines PDFs
- Video footage or b-roll

**Product/Brand Questions:**
- Product features and benefits
- Brand history and values
- Target audience insights
- Campaign goals and KPIs

## 3.3 Rate Card as Negotiation Baseline

### Your Rate Card Purpose

**Serves As:**
- Starting point for all negotiations
- Published rates visible to brands on your profile
- Brands see your rates before making offers
- Counter-offers reference your listed rates
- Establishes your value and professional standing

### Rate Card Location
- Public media kit at `/media-kit/:slug`
- Visible on your creator profile
- Included in media kit PDF
- Brands see this during discovery

### Benchmarking Your Rates

**Use API to Verify Rates:**
- `GET /api/v1/market-rates`
- Compare by:
  - Niche/category (Fashion, Tech, Beauty, etc.)
  - Tier (Micro, Mid, Macro, Mega)
  - Platform (Instagram, YouTube, TikTok)
  - Content type (Post, Reel, Story, Video)
  - Geography (US rates vs. international)

**Market Rate Data:**
```
Your Profile: Mid-tier Beauty creator, 50K followers, 4.5% engagement
Market Rate for Instagram Post: $800-$1,200
Your Rate Card: $1,000 (within range ✓)
```

### Adjusting Rates Based On

**Campaign Complexity:**
- Simple post: Standard rate
- Tutorial/how-to: +25% (more production)
- Unboxing: +15% (product showcase)
- Multi-platform: +30-50% (cross-posting)

**Demand Level:**
- High demand (booking 4+ weeks out): Increase 15-25%
- Low season: Consider slight discount for high-profile brands
- Rush campaigns: +25-50% rush premium

**Brand Prestige:**
- High-profile brand (portfolio value): Accept at rate card
- Startup/small brand: Flexible, consider future potential
- Dream collaboration: May accept slightly below rate card

### Negotiation Outcomes

**Possible Results:**

1. **Agreement Reached** ✅
   - Terms finalized
   - Contract generated
   - Move to Phase 4 (Contract & Commitment)

2. **No Agreement** ❌
   - Parties too far apart
   - Offer expires or is rejected
   - Back to Phase 1 (Discovery)

3. **Partial Agreement** 🔄
   - Some terms agreed, others pending
   - Continue negotiation
   - Platform suggests mediation after 3+ rounds

## Database Models (Inferred)

### NegotiationHistory Model
```
NegotiationHistory {
  id: uuid
  offerId: foreign key (DirectOffer or Collaboration)
  round: integer
  initiatedBy: enum (CREATOR, BRAND)
  originalTerms: JSON
  proposedTerms: JSON
  changes: JSON (diff between original and proposed)
  message: text (explanation/justification)
  status: enum (PENDING, ACCEPTED, REJECTED, COUNTERED)
  createdAt: timestamp
  respondedAt: timestamp (nullable)
}
```

### Message Model
```
Message {
  id: uuid
  conversationId: foreign key
  senderId: foreign key (userId)
  recipientId: foreign key (userId)
  messageType: enum (TEXT, FILE, IMAGE, GIF, VOICE, VIDEO)
  content: text
  fileUrl: string (nullable, for attachments)
  isRead: boolean
  readAt: timestamp (nullable)
  createdAt: timestamp
}
```

## API Endpoints (Inferred)

### Negotiation
- `GET /api/v1/direct-offers/:id/negotiation-history` - Full negotiation thread
- `POST /api/v1/direct-offers/:id/counter` - Send counter-offer
- `PATCH /api/v1/direct-offers/:id/accept-counter` - Accept counter-offer
- `GET /api/v1/collaborations/:id/negotiations` - Collaboration negotiation history

### Messaging
- `GET /api/v1/messages/conversations` - All conversations
- `GET /api/v1/messages/:conversationId` - Get message thread
- `POST /api/v1/messages` - Send message
- `POST /api/v1/messages/attachment` - Upload attachment
- `PATCH /api/v1/messages/:id/read` - Mark as read
- `POST /api/v1/messages/video-call` - Initiate video call
- `POST /api/v1/messages/voice-call` - Initiate voice call

### Market Rates
- `GET /api/v1/market-rates` - Get benchmark rates
  - Query params: niche, tier, platform, contentType, geography
  - Response: { averageRate, rangeMin, rangeMax, topCreatorRate }

## WebSocket Events (Socket.io)

### Real-Time Events
- `message:new` - New message received
- `message:read` - Message was read by recipient
- `typing:start` - User started typing
- `typing:stop` - User stopped typing
- `presence:online` - User came online
- `presence:offline` - User went offline
- `negotiation:update` - Negotiation status changed
- `offer:updated` - Offer terms changed

## Best Practices

### Counter-Offering
✅ **DO:**
- Know your market rate before negotiating
- Justify your rate with data (engagement, past ROI, testimonials)
- Be specific about what you'll deliver for your rate
- Offer alternatives (fewer deliverables for lower budget)
- Respond to counter-offers within 48 hours
- Keep tone professional and collaborative
- Be willing to meet in the middle

❌ **DON'T:**
- Counter with unrealistic amounts (5x original offer)
- Be confrontational or demanding
- Make ultimatums ("take it or leave it")
- Ignore brand's budget constraints
- Negotiate indefinitely (3-4 rounds max)
- Accept lowball offers that undervalue your work
- Burn bridges with aggressive negotiation

### Messaging
✅ **DO:**
- Respond within 24 hours (shows professionalism)
- Ask clarifying questions early
- Be friendly and professional
- Use video calls for complex discussions
- Keep messages focused and clear
- Thank brand for their time and interest

❌ **DON'T:**
- Ghost brands mid-conversation
- Send one-word responses
- Be overly casual or unprofessional
- Argue aggressively over terms
- Spam multiple messages
- Take days to respond

### Rate Setting
✅ **DO:**
- Benchmark rates quarterly (`/api/v1/market-rates`)
- Increase rates as you grow (follower milestones)
- Factor in production costs and time
- Charge premium for rush campaigns
- Negotiate exclusivity premiums separately
- Consider brand prestige and portfolio value

❌ **DON'T:**
- Undervalue your work consistently
- Set rates far above market average (unless top-tier)
- Forget to update rate card as you grow
- Accept every campaign at lower rates
- Ignore competitor pricing
- Set same rate for all content types (posts ≠ videos)
