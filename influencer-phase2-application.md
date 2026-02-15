# Phase 2: Application & Initial Response

## 2.1 Apply to a Campaign (Creator-Initiated)

### Route
**Entry**: `/app/marketplace` → Select campaign → Click "Quick Apply"

### Application Flow

**Step 1: Review Campaign Details**
- Full campaign description
- Deliverables breakdown (2 posts, 3 stories, 1 reel)
- Budget amount ($X - $Y range or fixed)
- Timeline (start, deadline, publish date, end)
- Brand guidelines and requirements
- Usage rights and exclusivity terms
- Approval process

**Step 2: Click "Quick Apply"**
- System calls `POST /api/v1/campaigns/:campaignId/apply`
- Opens application modal/form

**Step 3: Complete Application**

**Required Fields:**
- Confirmation of eligibility
- Acknowledgment of terms

**Optional Fields:**
- **Personal Message**: Text area (150-500 chars recommended)
  - Introduce yourself
  - Explain why you're a perfect fit
  - Reference specific aspects of the brand
  - Highlight relevant past work
- **Proposed Rate**: Can differ from campaign's listed budget
  - Auto-filled with your rate card pricing
  - Adjust if needed based on deliverables

**Step 4: Submit Application**
- Click "Submit Application"
- Application status: `PENDING`

### What Happens Next

**Brand Side:**
- Brand receives notification of your application
- Brand reviews your:
  - Profile
  - Media kit
  - Portfolio
  - Past campaign ratings
  - Influencer score
  - Application message
- Typical review time: 24-72 hours

### Possible Outcomes

**✅ APPROVED**
- Brand approves your application
- Brand creates a formal contract
- You receive notification to review contract
- Move to Phase 4 (Contract & Commitment)

**❌ REJECTED**
- Application declined
- Optional: Brand provides rejection reason
- Try other campaigns
- Learn from feedback for next application

**🔄 WITHDRAWN**
- You cancel your application before brand decision
- Can withdraw anytime while status is `PENDING`

### Tips for Strong Applications

**Content of Personal Message:**
- Use AI Pitch Writer (`/api/v1/pitch-writer`) for help
- Reference specific campaign goals
- Mention why your audience is a perfect match
- Highlight 1-2 relevant past collaborations
- Show enthusiasm for the brand
- Keep it concise (150-300 words ideal)

**Example Strong Message:**
```
Hi [Brand]! I'm excited about your spring campaign. My audience
is 75% female, 18-35, highly engaged with beauty content (5.8%
avg engagement). I recently partnered with [Similar Brand] and
drove 850 conversions through a similar campaign. I love your
brand's authentic approach and would create content that
resonates with both our audiences. Looking forward to
collaborating!
```

**Rate Proposal:**
- Propose within 10-20% of your rate card
- If campaign budget is below your rates, explain value (don't undersell)
- Consider accepting slightly lower for high-profile brands (portfolio value)
- Factor in deliverables (more content = higher rate)

## 2.2 Respond to a Direct Offer (Brand-Initiated)

### Route
**Primary**: `/app/direct-offers` → Select offer

### Offer Details Review

**Information to Review:**
- **Budget amount**: Total compensation
- **Deliverables**: Content types, quantities, platforms
  - Example: 2 Instagram posts, 5 stories, 1 TikTok video
- **Timeline**:
  - Start date
  - Content submission deadline
  - Publishing date
  - Campaign end date
- **Content Guidelines**: Brand's messaging requirements
- **Required Elements**: Hashtags, mentions, disclosures
- **Usage Rights**: How brand can use your content
- **Exclusivity Terms**: Competitor restrictions
- **Brand's Personal Message**: Why they chose you
- **Expiry Countdown**: Days remaining (7-day default)

### Three Response Options

#### ✅ Accept Offer

**Process:**
1. Click "Accept" button
2. Contract preview modal appears
3. Review auto-generated contract terms
4. Confirm acceptance
5. API: `PATCH /api/v1/direct-offers/:id/accept`
6. Offer auto-converts to formal contract
7. Redirects to `/app/contracts/:contractId`
8. **Status Change**: `ACCEPTED` → `CONVERTED_TO_CONTRACT`

**Result:**
- Contract created and ready for e-signature
- Move to Phase 4 (Contract & Commitment)

#### 💬 Counter-Offer

**Process:**
1. Click "Counter-Offer" button
2. Counter-offer form opens

**Form Fields:**
- **Counter Budget**: Enter your proposed amount (minimum $100)
  - Validation: Must be reasonable (not 10x original offer)
- **Counter Message**: Explain your reasoning (required, 100+ chars)
  - Why you're worth more
  - Highlight specific value you bring
  - Reference your rate card
  - Suggest alternative deliverables
- **Alternative Deliverables** (optional): Suggest changes to scope

3. Submit counter-offer
4. API: `PATCH /api/v1/direct-offers/:id/counter`
5. **Status Change**: `PENDING` → `NEGOTIATING`
6. `negotiationRound` increments (1, 2, 3...)

**Brand Response Options:**
- Accept your counter (→ contract created)
- Send new counter-offer (→ you review and respond)
- Reject counter (→ offer ends)

**Result:**
- Move to Phase 3 (Negotiation & Terms)

#### ❌ Reject Offer

**Process:**
1. Click "Reject" button
2. Rejection form opens

**Form Fields:**
- **Rejection Reason** (optional dropdown):
  - Budget too low
  - Timeline doesn't work
  - Not a good brand fit
  - Already committed to similar brand
  - Other (specify)
- **Rejection Message** (optional): Polite explanation

3. Confirm rejection
4. API: `PATCH /api/v1/direct-offers/:id/reject`
5. **Status Change**: `PENDING` → `REJECTED`

**Result:**
- Offer is declined
- Brand is notified
- You can continue browsing other opportunities

### Direct Offer Status Lifecycle
```
PENDING → VIEWED (when you open it)
        → ACCEPTED → CONVERTED_TO_CONTRACT
        → REJECTED
        → NEGOTIATING → ACCEPTED / REJECTED
        → EXPIRED (after 7 days)
```

## 2.3 Respond to Collaboration Proposals

### Route
**Primary**: `/app/tools/collaborations`

### Collaboration Tabs

**Tab Organization:**
- **Active**: Ongoing collaborations
- **Pending**: Awaiting your response
- **Completed**: Past collaborations

### Collaboration vs. Direct Offer

**Key Differences:**
- Collaborations may be less formal
- Can involve non-monetary partnerships (product seeding, affiliate deals)
- Often more flexible terms
- May not have strict contract (depends on brand)

### Actions on Collaboration Proposals

**✅ Accept**
- Accept collaboration proposal
- Proceeds to agreement phase
- May or may not involve formal contract

**❌ Decline**
- Decline proposal with optional reason
- Professional way to pass on opportunity

**💬 Negotiate**
- Send counter-offer with message
- Propose alternative terms:
  - Different deliverables
  - Modified timeline
  - Adjusted compensation
  - Usage rights changes

**Backend**: `/api/v1/collaborations` via `collaborationService`

## Database Models (Inferred)

### CampaignApplication Model
```
CampaignApplication {
  id: uuid
  campaignId: foreign key
  creatorId: foreign key
  personalMessage: text
  proposedRate: decimal
  status: enum (PENDING, APPROVED, REJECTED, WITHDRAWN)
  appliedAt: timestamp
  reviewedAt: timestamp (nullable)
  reviewedBy: userId (nullable)
  rejectionReason: text (nullable)
}
```

### DirectOfferResponse Model
```
DirectOfferResponse {
  id: uuid
  offerId: foreign key
  responseType: enum (ACCEPT, REJECT, COUNTER)
  counterBudget: decimal (nullable, for COUNTER)
  counterMessage: text (nullable, for COUNTER)
  rejectionReason: enum (nullable, for REJECT)
  rejectionMessage: text (nullable, for REJECT)
  respondedAt: timestamp
}
```

### Collaboration Model
```
Collaboration {
  id: uuid
  brandId: foreign key
  creatorId: foreign key
  title: string
  description: text
  proposalType: enum (PAID, PRODUCT_SEEDING, AFFILIATE, HYBRID)
  budget: decimal (nullable)
  deliverables: JSON
  status: enum (PENDING, ACCEPTED, DECLINED, NEGOTIATING, ACTIVE, COMPLETED)
  createdAt: timestamp
}
```

## API Endpoints (Inferred)

### Campaign Applications
- `POST /api/v1/campaigns/:campaignId/apply` - Submit application
- `GET /api/v1/campaigns/applications` - Get your applications
- `GET /api/v1/campaigns/applications/:id` - Get application details
- `PATCH /api/v1/campaigns/applications/:id/withdraw` - Withdraw application
- `GET /api/v1/campaigns/:campaignId/status` - Check application status

### Direct Offer Responses
- `PATCH /api/v1/direct-offers/:id/accept` - Accept offer
- `PATCH /api/v1/direct-offers/:id/reject` - Reject offer
- `PATCH /api/v1/direct-offers/:id/counter` - Send counter-offer
- `GET /api/v1/direct-offers/:id/negotiation-history` - View negotiation thread

### Collaborations
- `GET /api/v1/collaborations` - Get all collaboration proposals
- `GET /api/v1/collaborations/:id` - Get collaboration details
- `PATCH /api/v1/collaborations/:id/accept` - Accept collaboration
- `PATCH /api/v1/collaborations/:id/decline` - Decline collaboration
- `PATCH /api/v1/collaborations/:id/negotiate` - Negotiate terms

### AI Pitch Writer
- `POST /api/v1/pitch-writer` - Generate pitch for campaign
  - Request body: { campaignId, creatorProfile, targetBrand }
  - Response: { suggestedPitch, keyPoints, toneRecommendation }

## Best Practices

### Campaign Applications
✅ **DO:**
- Apply within 24 hours of campaign posting
- Write personalized messages (reference specific campaign details)
- Use AI Pitch Writer for inspiration (but customize)
- Propose rate within 20% of your rate card
- Highlight 1-2 most relevant past campaigns
- Show enthusiasm for the brand
- Apply to 5-10 campaigns weekly

❌ **DON'T:**
- Send generic copy-paste applications
- Apply to campaigns outside your niche
- Lowball your rates just to get selected
- Write novels (keep message 150-300 words)
- Forget to proofread (typos hurt professionalism)
- Apply to competing brands simultaneously

### Direct Offer Responses
✅ **DO:**
- Respond within 24 hours (shows professionalism)
- Review all terms carefully before accepting
- Counter-offer professionally with justification
- Check brand's profile and past creator reviews
- Ask clarifying questions if anything is unclear
- Be polite even when rejecting

❌ **DON'T:**
- Let offers expire (7-day limit)
- Accept every offer without evaluation
- Counter-offer with unrealistic amounts
- Reject without explanation (burns bridges)
- Ghost brands after viewing offer
- Accept and then back out later

### Negotiation Tips
✅ **DO:**
- Know your market rate (`/api/v1/market-rates`)
- Justify your rate with data (engagement, past ROI)
- Be flexible on timeline if budget is good
- Suggest alternative deliverables if budget is firm
- Keep negotiations professional and friendly
- Respond to counter-offers within 48 hours

❌ **DON'T:**
- Be confrontational or demanding
- Make ultimatums
- Counter-offer more than 2-3 times (find middle ground)
- Ignore budget constraints (brands have limits)
- Negotiate on every single term (pick your battles)
