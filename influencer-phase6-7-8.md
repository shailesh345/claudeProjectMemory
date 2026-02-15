# Phases 6-8: Submission, Publishing & Payment

## Phase 6: Submission & Review

### 6.1 Submit Content for Approval

**API**: `POST /api/v1/content-submissions`

**Submission Form Fields:**
- **Content Type**: Post, Reel, Story, Video, IGTV, etc.
- **Content URL**: Link to draft or preview (if already on platform)
- **Media URL**: Direct link to media files (MP4, JPG, PNG)
- **Captions/Script**: Full text content with hashtags and mentions
- **Notes to Brand**: Context, creative choices, any explanations

**Initial Status**: `PENDING_REVIEW`
**Revision Number**: Starts at `0`

### 6.2 Brand Reviews Your Content

**Three Possible Outcomes:**

#### ✅ Approved
- Brand clicks "Approve"
- You receive approval notification (email + in-app)
- Content cleared for publishing
- **Status**: `PENDING_REVIEW` → `APPROVED`
- **Move to**: Phase 7 (Publishing)

#### 🔄 Revision Requested
- Brand clicks "Request Revision"
- Brand specifies changes needed:
  - Caption/text adjustments
  - Visual/editing changes
  - Missing hashtags or mentions
  - Brand guidelines not followed
  - Posting time/date change
  - Other specific feedback
- You receive detailed feedback
- **Status**: `PENDING_REVIEW` → `REVISION_REQUESTED`
- **Action**: Make changes and resubmit

#### ❌ Rejected
- Rare, for serious issues only
- Requires brand explanation
- May trigger dispute resolution
- **Status**: `PENDING_REVIEW` → `REJECTED`
- **Action**: Review rejection reason, possibly initiate dispute

### 6.3 Handle Revisions

**Revision Process:**
1. Review brand's specific feedback carefully
2. Make requested changes only (don't change other things)
3. Resubmit via `POST /api/v1/content-submissions`
4. `revisionNumber` increments (1 → 2 → 3)
5. Brand reviews again

**Revision Expectations:**
- **Typically**: 1-2 rounds are standard
- **Contract may specify**: "Up to 3 revisions included"
- **After limit**: Additional revisions may incur extra fees
- **Feedback must be specific**: Vague feedback may warrant pushback

**Submission Status Lifecycle:**
```
PENDING_REVIEW → APPROVED (ready to publish)
               → REVISION_REQUESTED → (resubmit) → PENDING_REVIEW
               → REJECTED
```

---

## Phase 7: Publishing & Go-Live

### 7.1 Publish Content

**Publishing Checklist:**
- [ ] Content is brand-approved
- [ ] All required hashtags included (#ad, #sponsored first, then brand hashtags)
- [ ] All required @mentions included
- [ ] FTC disclosure present and VISIBLE (no "see more")
- [ ] Link in bio updated (if required)
- [ ] Posted at agreed time/date
- [ ] Correct platform selected
- [ ] Content quality matches approved version exactly
- [ ] No unauthorized changes from approved version

**Publishing Options:**
- **Manual**: Post directly on each social platform (Instagram, TikTok, YouTube)
- **Cross-Platform**: Use `/api/v1/cross-platform` to publish to multiple platforms
- **Scheduled**: Use content queue for timed publishing

**After Publishing:**
1. Copy the live post URL
2. Mark content as "Published" in campaign dashboard
3. Provide post link to brand via platform
4. Brand receives notification: "Content is live!"

**Status**: `APPROVED` → `PUBLISHED - Live & Tracking`

### 7.2 Content Verification

**Self-Check (Before Brand Verifies):**
- ✅ Correct hashtags present and visible
- ✅ Correct @mentions tagged
- ✅ FTC disclosure visible (#ad or #sponsored in first 3 hashtags)
- ✅ Link in bio set and working (if required)
- ✅ Posted at agreed time (+/- 30 minutes acceptable)
- ✅ Content matches approved version exactly
- ✅ Correct platform
- ✅ No errors or typos

**Brand Verifies:**
- Brand visits live post
- Checks all requirements met
- Confirms content is live and correct
- Approves milestone for payment release

### 7.3 Track Performance

**Metrics Tracked Automatically:**
- 👁️ **Views/Impressions**: Total reach
- ❤️ **Likes**: Engagement count
- 💬 **Comments**: Comment count
- 🔄 **Shares**: Shares/retweets
- 💾 **Saves**: Bookmarks/saves
- 📊 **Engagement Rate**: (Likes + Comments + Shares) / Reach
- 🔗 **Link Clicks**: UTM-tracked link clicks
- 🛒 **Conversions**: E-commerce sales (if integrated)

**Analytics Available:**

**Content Analytics**: `/app/analytics`
- Individual post performance
- Engagement breakdown
- Audience demographics

**Creator Analytics**: `/api/v1/creator-analytics`
- Career-wide metrics
- Growth trends
- Average engagement rates

**Profile Analytics**: `/app/profile-analytics`
- Profile insights
- Follower growth
- Traffic sources

**Platform CPM**: `/api/v1/platform-cpm`
- Revenue per platform
- Cost per thousand impressions

**ROI Calculator**: `/api/v1/roi`
- Campaign return on investment
- Performance vs. goals

**Performance Data Benefits:**
- Feeds into brand's campaign analytics report
- Improves your influencer score
- Builds track record for future campaigns
- Helps you optimize future content

**Status**: `Live - Tracking`

---

## Phase 8: Payment & Collection

### 8.1 Milestone Completion & Escrow Release

**Route**: `/app/escrow/:campaignId`

**Payment Flow:**

1. **Approved Content Triggers Milestone**
   - Each approved & published submission completes a milestone
   - Milestone status: `SUBMITTED` → `APPROVED` → `PAID`

2. **Brand Approves Milestone**
   - Brand reviews performance
   - Clicks "Approve Milestone" in escrow dashboard
   - Approval triggers automatic payment release

3. **Escrow Releases Funds**
   - Funds released from escrow to your wallet
   - Platform fee (10%) deducted automatically
   - Example: $1,000 milestone - $100 fee = $900 to your wallet

4. **You Receive Payment**
   - Funds appear in wallet balance
   - Email notification: "Payment received for [Campaign]"
   - Invoice auto-generated

**Escrow Milestone Flow:**
```
PENDING (not started)
  ↓
IN_PROGRESS (working on it)
  ↓
SUBMITTED (submitted for review)
  ↓
APPROVED (brand approved)
  ↓
PAID (funds in your wallet) ✓
```

**If Brand Doesn't Respond:**
- **Wait 7 days**: Give brand reasonable time
- **Contact brand**: Via `/app/messages` - polite reminder
- **14 days**: Platform auto-reminder sent to brand
- **21 days**: Initiate dispute via `/api/v1/disputes`
- **28 days**: Platform may auto-release if no response

**Dispute Process:**
- **Initiate**: Click "Dispute Milestone" in escrow dashboard
- **Evidence**: Submit screenshots, messages, proof of completion
- **Brand Response**: Brand submits counter-evidence
- **Platform Mediation**: Admin reviews both sides
- **Resolution**: Binding decision within 5-7 business days

### 8.2 Wallet Management

**Route**: `/app/wallet`

**Wallet Dashboard Shows:**
- **Total Balance**: Available funds you can withdraw
- **Locked Balance**: Funds in active escrows (pending milestones)
- **Total Earned**: Lifetime earnings
- **Total Deposited**: Any deposits you made
- **Total Withdrawn**: Total withdrawn to bank

**Transaction Types:**
- `EARNING` - Campaign payment received
- `ESCROW_RELEASE` - Funds released from escrow
- `WITHDRAWAL` - Transferred to bank account
- `DEPOSIT` - Funds you added
- `PAYMENT` - Platform payments/fees
- `TRANSFER` - Internal transfers
- `REFUND` - Refunded amounts
- `ESCROW_LOCK` - Funds locked in escrow

**Add Credits**: `/app/wallet/add-credits`
- Stripe payment integration
- Add funds for platform features (Spotlight, etc.)

### 8.3 Withdraw Funds

**Prerequisites:**
- ✅ KYC verification completed (`/api/v1/kyc`)
- ✅ Stripe Connect account linked (`stripeConnectId`)
- ✅ Minimum balance: $50 (or platform minimum)

**Withdrawal Process:**
1. Navigate to `/app/wallet`
2. Click "Withdraw" button
3. Enter withdrawal amount (up to available balance)
4. Confirm bank account details
5. Review withdrawal summary:
   - Amount: $1,000
   - Transfer fee: $0 (free)
   - You'll receive: $1,000
6. Click "Confirm Withdrawal"
7. Funds transferred via Stripe Connect
8. **Timeline**: 2-5 business days to arrive in bank

**Backend**: `/api/v1/wallet/withdraw`

**Withdrawal Limits:**
- **Minimum**: $50 per withdrawal
- **Maximum**: $10,000 per transaction (contact support for higher)
- **Frequency**: Unlimited (no monthly limit)

### 8.4 Invoice Management

**Route**: `/app/invoices`

**Invoice Features:**
- Create professional invoices for completed work
- Auto-populate from campaign/contract details
- Export as PDF for your records
- Track payment status

**Invoice Fields:**
- **Invoice Number**: Auto-generated (unique)
- **Recipient**: Brand name and details
- **Line Items**: Deliverables completed
  - Instagram Posts (2) - $800
  - Instagram Stories (3) - $400
  - TikTok Reel (1) - $600
- **Subtotal**: $1,800
- **Platform Fee (10%)**: -$180
- **Total**: $1,620
- **Status**: Draft, Sent, Paid, Overdue

**Backend**: `/api/v1/invoices`

**Invoice Actions:**
- Create new invoice
- Send to brand (email)
- Mark as paid (when escrow releases)
- Download PDF
- Resend invoice

---

## Database Models (Inferred)

### ContentSubmission
```
ContentSubmission {
  id: uuid
  contractId: foreign key
  milestoneId: foreign key
  creatorId: foreign key
  platform: enum
  contentType: enum
  fileUrls: array (media files)
  contentUrl: string (live post URL if published)
  caption: text
  hashtags: array
  mentions: array
  notes: text
  revisionNumber: integer
  status: enum (PENDING_REVIEW, APPROVED, REVISION_REQUESTED, REJECTED)
  submittedAt: timestamp
  reviewedAt: timestamp
  approvedAt: timestamp
}
```

### ContentReview
```
ContentReview {
  id: uuid
  submissionId: foreign key
  reviewedBy: userId (brandId)
  action: enum (APPROVE, REQUEST_REVISION, REJECT)
  feedback: text
  changeCategories: array
  priority: enum (MINOR, MODERATE, CRITICAL)
  reviewedAt: timestamp
}
```

### Wallet
```
Wallet {
  id: uuid
  userId: foreign key
  balance: decimal
  lockedBalance: decimal
  totalEarned: decimal
  totalDeposited: decimal
  totalWithdrawn: decimal
  currency: string (USD, EUR, etc.)
  stripeConnectId: string
  updatedAt: timestamp
}
```

### WalletTransaction
```
WalletTransaction {
  id: uuid
  walletId: foreign key
  type: enum (EARNING, ESCROW_RELEASE, WITHDRAWAL, DEPOSIT, PAYMENT, TRANSFER, REFUND, ESCROW_LOCK)
  amount: decimal
  balance: decimal (after transaction)
  description: text
  relatedEntityId: uuid (contractId, escrowId, etc.)
  createdAt: timestamp
}
```

### Invoice
```
Invoice {
  id: uuid
  creatorId: foreign key
  brandId: foreign key
  contractId: foreign key
  invoiceNumber: string (unique)
  lineItems: JSON array
  subtotal: decimal
  platformFee: decimal
  tax: decimal (if applicable)
  total: decimal
  status: enum (DRAFT, SENT, PAID, OVERDUE)
  issuedAt: timestamp
  paidAt: timestamp
  fileUrl: string (PDF)
}
```

---

## API Endpoints (Inferred)

### Content Submission
- `POST /api/v1/content-submissions` - Submit content
- `GET /api/v1/content-submissions/:id` - Get submission details
- `GET /api/v1/contracts/:contractId/submissions` - All submissions for contract
- `GET /api/v1/submissions/:id/history` - Revision history

### Publishing
- `POST /api/v1/content/publish` - Manual publish
- `POST /api/v1/cross-platform/publish` - Multi-platform publish
- `POST /api/v1/queue/publish/:id` - Publish scheduled content
- `POST /api/v1/contracts/:id/mark-published` - Mark as published

### Analytics
- `GET /api/v1/analytics` - Content analytics
- `GET /api/v1/creator-analytics` - Creator-wide metrics
- `GET /api/v1/profile-analytics` - Profile insights
- `GET /api/v1/platform-cpm` - Platform CPM data
- `GET /api/v1/roi` - ROI calculation

### Wallet
- `GET /api/v1/wallet` - Get wallet details
- `GET /api/v1/wallet/transactions` - Transaction history
- `POST /api/v1/wallet/withdraw` - Initiate withdrawal
- `POST /api/v1/wallet/add-credits` - Add credits
- `GET /api/v1/wallet/balance` - Check balance

### Invoices
- `GET /api/v1/invoices` - Get all invoices
- `POST /api/v1/invoices` - Create invoice
- `GET /api/v1/invoices/:id` - Get invoice details
- `PUT /api/v1/invoices/:id` - Update invoice
- `POST /api/v1/invoices/:id/send` - Send to brand
- `GET /api/v1/invoices/:id/download` - Download PDF

### Disputes
- `POST /api/v1/disputes` - Create dispute
- `POST /api/v1/disputes/:id/evidence` - Submit evidence
- `GET /api/v1/disputes/:id` - Get dispute status
- `POST /api/v1/disputes/:id/message` - Message in dispute thread

---

## Best Practices

### Submission
✅ **DO:**
- Submit early (don't wait until deadline)
- Double-check all requirements before submitting
- Include helpful notes for brand
- Self-review thoroughly first
- Submit high-quality files (full resolution)

❌ **DON'T:**
- Submit at last minute
- Skip checking hashtags/mentions
- Submit low-quality drafts
- Forget FTC disclosures
- Submit without reviewing guidelines

### Revisions
✅ **DO:**
- Address feedback specifically
- Make only requested changes
- Resubmit promptly (within 24-48 hours)
- Ask clarifying questions if feedback is vague
- Stay professional and open to feedback

❌ **DON'T:**
- Argue about reasonable feedback
- Ignore specific feedback
- Make unrelated changes
- Take feedback personally
- Submit unchanged content

### Publishing
✅ **DO:**
- Verify all requirements before posting
- Post at agreed time
- Provide post link immediately
- Engage with comments
- Monitor performance

❌ **DON'T:**
- Publish without approval
- Miss posting deadline
- Delete or edit post without brand permission
- Remove FTC disclosures after posting
- Ignore brand's questions about performance

### Payment
✅ **DO:**
- Wait reasonable time for brand review (7 days)
- Follow up politely if delayed
- Maintain evidence of completed work
- Withdraw regularly to avoid large balance
- Keep invoices for tax records

❌ **DON'T:**
- Harass brand for immediate payment
- Initiate dispute without communication first
- Spend earnings before escrow releases
- Forget to download invoices for taxes
- Ignore payment discrepancies
