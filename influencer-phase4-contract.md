# Phase 4: Contract & Commitment

## 4.1 Review Contract

### Route
**Primary**: `/app/contracts/:contractId`

### Contract Status on Arrival
**Initial Status**: `DRAFT` or `PENDING_CREATOR`

### Contract Sections to Review Thoroughly

#### 1. Deliverables

**Review:**
- **Exact content pieces required**: "2 Instagram posts, 3 Stories, 1 Reel"
- **Content type per deliverable**: Post, Reel, Story, Video, IGTV
- **Platform per deliverable**: Instagram, YouTube, TikTok, etc.
- **Quality standards**: Resolution, editing requirements
- **Specifications**: Video length (15-30s), image dimensions, etc.

**Verify:**
- Deliverables match what you agreed to in negotiation
- You can realistically create all required content
- Timeline allows for quality production

#### 2. Financial Terms

**Total Amount:**
- Agreed compensation (lump sum or per deliverable)
- Currency (USD, EUR, etc.)
- Breakdown if multi-deliverable

**Payment Terms:**
- **Upfront (50% before, 50% after)**: Half paid before work starts
- **Upon completion**: Full payment after all deliverables approved
- **Net 30/60/90**: Payment 30/60/90 days after completion
- **Milestone-based**: Payment tied to individual deliverable approvals

**Milestones (if applicable):**
- Individual deliverable milestones
- Amount per milestone
- Due date per milestone
- Approval criteria

**Example Milestone Breakdown:**
```
Milestone 1: 2 Instagram Posts - $800 - Due: March 15
Milestone 2: 3 Instagram Stories - $400 - Due: March 18
Milestone 3: 1 TikTok Reel - $600 - Due: March 20
Total: $1,800
```

#### 3. Timeline

**Key Dates:**
- **Start Date**: When campaign work officially begins
- **Content Submission Deadline**: When you must submit for review
- **Publishing Date**: When content goes live
- **Campaign End Date**: When tracking period ends

**Verify:**
- Submission deadline is realistic for your workload
- Adequate time between submission and publishing (for revisions)
- No conflicts with other campaigns

#### 4. Content Guidelines

**Messaging Requirements:**
- **Key talking points**: Specific messages to convey
- **Tone and style**: Professional, casual, humorous, inspirational
- **Do's and Don'ts**: What to include/avoid

**Brand Assets Provided:**
- **Logo**: Hi-res brand logo files
- **Product images**: Official product photos
- **Brand guidelines PDF**: Full brand book
- **Color palette**: Hex codes for brand colors
- **Fonts**: Brand typography

#### 5. Required Elements

**Mandatory Inclusions:**
- **Hashtags**: List of required hashtags
  - #ad, #sponsored (FTC compliance)
  - Brand-specific hashtags
  - Campaign hashtags
- **Mentions**: Brand @handles to tag
  - Primary brand account
  - Campaign-specific accounts
- **Disclosures**: FTC compliance requirements
  - #ad or #sponsored must be in first 3 hashtags
  - Disclosure must be visible without "see more" click
- **Links**: URLs to include (UTM-tracked)
  - Link in bio
  - Swipe-up links (if available)
  - Trackable campaign URL

#### 6. Rights & Exclusivity

**Usage Rights:**
- **How brand can reuse your content**:
  - Social media only (Instagram, Facebook, Twitter)
  - Social + website (blog, product pages)
  - Social + website + paid advertising (Facebook Ads, Google Ads)
  - All media (TV, print, outdoor, digital)

**Usage Rights Duration:**
- **30 days**: Short-term usage
- **90 days**: Quarter-long usage
- **1 year**: Annual license
- **Perpetual**: Forever (commands 2-3x premium)

**Exclusivity Period:**
- **Category**: Can't promote competitors in [category] for X months
- **Duration**: 3 months, 6 months, 12 months
- **Scope**: Specific competitors or all brands in category
- **Example**: "Cannot promote skincare brands for 6 months after campaign"

**Content Ownership:**
- **Creator retains copyright**: You own the raw files
- **Brand has usage rights**: Per terms above
- **Raw files delivery**: Whether brand gets original files or just published content

### Red Flags to Watch For

❌ **Unfair Terms:**
- Perpetual usage rights without significant premium
- Unlimited revisions
- Extremely short timelines (24-48 hours)
- No payment until 90+ days after completion
- Vague deliverables ("create content as needed")
- Hidden fees or deductions
- No escrow protection

## 4.2 Sign Contract Digitally

### E-Signature Process

**Step-by-Step:**

1. **Review Contract Thoroughly**
   - Read every section carefully
   - Verify all terms match negotiations
   - Check dates, amounts, deliverables
   - Ensure no surprise clauses

2. **Click "Sign Contract" Button**
   - Located at bottom of contract page
   - May require scrolling to bottom (ensuring you read everything)

3. **E-Signature Capture**
   - Type your full legal name
   - Draw signature (if signature pad available)
   - Check "I agree to the terms" checkbox

4. **System Records Your Signature**
   - `creatorSignedAt`: Timestamp of your signature
   - `creatorSignatureIP`: Your IP address at signing (legal proof)
   - `creatorSignature`: Encrypted signature data

5. **Brand Also Signs**
   - Brand receives notification to sign
   - `brandSignedAt`: Brand's signature timestamp
   - `brandSignatureIP`: Brand's IP address
   - `brandSignature`: Brand's signature data

6. **Contract Activation**
   - **Both signatures required** to activate contract
   - Once both signed: Status → `ACTIVE`
   - Legally binding agreement now in effect

### Contract Status Lifecycle
```
DRAFT → PENDING_CREATOR (awaiting your signature)
      → PENDING_BRAND (you signed, awaiting brand)
      → ACTIVE (both signed, campaign starts)
      → IN_PROGRESS (work underway)
      → COMPLETED (all milestones complete)
      → DISPUTED (if issues arise)
      → CANCELLED (if terminated early)
```

### What Happens After Both Sign

**Automatic Actions:**
1. You receive confirmation email
2. Brand receives confirmation email
3. Contract PDF generated and stored
4. Campaign officially starts
5. Escrow funding initiated (if applicable)
6. Calendar reminders sent for deadlines

## 4.3 Escrow Protection Activated

### Route
**Primary**: `/app/escrow/:campaignId`

### How Escrow Works

**Escrow Flow:**

1. **Brand Funds Escrow**
   - Before work begins, brand deposits full campaign amount
   - Funds locked in escrow account
   - Cannot be withdrawn by brand once locked

2. **Your Payment is Protected**
   - Payment is guaranteed and locked
   - Platform holds funds as neutral third party
   - Brand cannot back out once escrow is funded

3. **Neutral Third Party**
   - Platform manages escrow account
   - Not accessible by brand or creator until approval
   - Ensures fair payment for completed work

4. **Milestone-Based Releases**
   - Tied to individual deliverable approvals
   - Each approved milestone releases portion of payment
   - Example: Milestone 1 approved → $800 released to your wallet

5. **Platform Fee Deduction**
   - **10% platform fee** deducted automatically
   - Deducted from your earnings
   - Example: $1,000 milestone → $100 fee → You receive $900

### Escrow Status Lifecycle
```
PENDING (escrow created, awaiting funding)
  ↓
FUNDED (brand deposited funds)
  ↓
PARTIALLY_RELEASED (some milestones paid)
  ↓
RELEASED (all milestones paid)

Alternative flows:
  → DISPUTED (issue raised by either party)
  → REFUNDED (campaign cancelled, funds returned to brand)
  → CANCELLED (contract terminated)
```

### Milestone Status Lifecycle
```
PENDING (not started)
  ↓
IN_PROGRESS (you're working on it)
  ↓
SUBMITTED (you submitted for review)
  ↓
APPROVED (brand approved) → PAID (funds released to you)

OR

REJECTED (brand rejected) → Resubmit
```

### Escrow Dashboard Information

**Dashboard Shows:**
- **Total campaign amount**: $1,800
- **Amount funded by brand**: $1,800 ✓
- **Amount released to you**: $800 (Milestone 1 complete)
- **Amount remaining**: $1,000 (Milestones 2 & 3 pending)
- **Platform fee (10%)**: $180 (deducted upon release)
- **Your net earnings**: $1,620

**Individual Milestone Tracking:**
```
Milestone 1: Instagram Posts
  Status: PAID ✓
  Amount: $800
  You received: $720 (after $80 fee)

Milestone 2: Instagram Stories
  Status: SUBMITTED (awaiting brand review)
  Amount: $400
  You'll receive: $360 (after $40 fee)

Milestone 3: TikTok Reel
  Status: IN_PROGRESS
  Amount: $600
  You'll receive: $540 (after $60 fee)
```

**Actions Available:**
- View transaction history
- Dispute milestone (if issue arises)
- Download invoice
- Contact support

### Protection Guarantees

**What Escrow Protects:**

✅ **Guaranteed Payment**: Funds locked before you start work
✅ **No Brand Withdrawal**: Brand cannot pull funds once locked
✅ **Automatic Release**: Milestones release funds automatically upon approval
✅ **Dispute Resolution**: Platform mediates if issues arise
✅ **Non-Responsive Brands**: Auto-release if brand doesn't respond in 14 days
✅ **Fair Process**: Evidence-based dispute system

**Dispute Resolution Available:**
- Route: `/api/v1/disputes`
- Initiate if:
  - Brand won't approve completed work
  - Brand requests excessive revisions beyond contract
  - Brand stops responding
  - Scope creep (additional deliverables not in contract)

**Platform Mediation:**
- Admin reviews evidence from both sides
- Reviews contract terms
- Makes binding decision
- Funds released or refunded based on evidence

### Final Status
**Status**: `Contract Signed - Escrow Funded - Ready to Work`

**You're Protected and Ready to Create!**

## Database Models (Inferred)

### Contract Model
```
Contract {
  id: uuid
  campaignId: foreign key
  brandId: foreign key
  creatorId: foreign key
  deliverables: JSON
  financialTerms: JSON {
    totalAmount: decimal
    paymentTerms: enum
    milestones: array
  }
  timeline: JSON {
    startDate: date
    submissionDeadline: date
    publishDate: date
    endDate: date
  }
  guidelines: JSON
  usageRights: JSON {
    platforms: array
    duration: integer (days)
    exclusivityPeriod: integer (months)
  }
  status: enum (DRAFT, PENDING_CREATOR, PENDING_BRAND, ACTIVE, IN_PROGRESS, COMPLETED, DISPUTED, CANCELLED)
  creatorSigned: boolean
  creatorSignedAt: timestamp
  creatorSignatureIP: string
  brandSigned: boolean
  brandSignedAt: timestamp
  brandSignatureIP: string
  fullyExecuted: boolean (both signed)
  createdAt: timestamp
}
```

### Escrow Model
```
Escrow {
  id: uuid
  contractId: foreign key
  campaignId: foreign key
  totalAmount: decimal
  platformFee: decimal (10%)
  netToCreator: decimal
  status: enum (PENDING, FUNDED, PARTIALLY_RELEASED, RELEASED, DISPUTED, REFUNDED, CANCELLED)
  fundedAt: timestamp
  fundedAmount: decimal
  releasedAmount: decimal
  remainingAmount: decimal
}
```

### Milestone Model
```
Milestone {
  id: uuid
  contractId: foreign key
  escrowId: foreign key
  deliverableName: string
  description: text
  amount: decimal
  dueDate: date
  status: enum (PENDING, IN_PROGRESS, SUBMITTED, APPROVED, REJECTED, PAID)
  submittedAt: timestamp
  approvedAt: timestamp
  paidAt: timestamp
  rejectionReason: text (nullable)
}
```

## API Endpoints (Inferred)

### Contracts
- `GET /api/v1/contracts/:id` - Get contract details
- `POST /api/v1/contracts/:id/sign` - E-sign contract
- `GET /api/v1/contracts/:id/download` - Download PDF
- `GET /api/v1/contracts` - Get all your contracts
- `PATCH /api/v1/contracts/:id/status` - Update status

### Escrow
- `GET /api/v1/escrow/:campaignId` - Get escrow dashboard
- `GET /api/v1/escrow/:id/status` - Check escrow status
- `GET /api/v1/escrow/:id/milestones` - Get all milestones
- `POST /api/v1/escrow/:id/dispute` - Initiate dispute

### Milestones
- `PATCH /api/v1/milestones/:id/start` - Mark as in progress
- `POST /api/v1/milestones/:id/submit` - Submit for approval
- `GET /api/v1/milestones/:id/status` - Check milestone status

### Disputes
- `POST /api/v1/disputes` - Create dispute
- `POST /api/v1/disputes/:id/evidence` - Submit evidence
- `GET /api/v1/disputes/:id` - Get dispute details
- `GET /api/v1/disputes/:id/status` - Check resolution status

## Best Practices

### Contract Review
✅ **DO:**
- Read every section carefully (don't skim)
- Verify terms match negotiations
- Check dates are realistic
- Ensure deliverables are clear and achievable
- Save a copy for your records
- Ask questions before signing

❌ **DON'T:**
- Sign without reading thoroughly
- Assume terms match verbal agreements
- Accept vague deliverables
- Agree to unrealistic timelines
- Skip checking usage rights duration
- Sign if anything seems unfair

### Escrow Protection
✅ **DO:**
- Verify escrow is funded before starting work
- Check milestone breakdown matches contract
- Submit work on time to trigger releases
- Keep evidence of completed work
- Document all communications

❌ **DON'T:**
- Start work before escrow is funded
- Miss submission deadlines (delays payment)
- Submit incomplete work
- Agree to work outside escrow system
- Delete evidence (messages, drafts, files)
