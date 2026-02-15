# Phase 7: Campaign Completion & Payment

## Status Flow
```
Live - Tracking → Completed - Pending Payment → Completed - Paid → Completed
```

## Campaign Ends

### Automatic Trigger
- Campaign end date reached (defined in Phase 3)
- Final metrics captured from all platforms
- No more tracking updates after this point

### Automatic Actions
1. **Final Metrics Snapshot**
   - Last API call to all platforms
   - Save final counts for all metrics
   - Calculate final ROI
   - Lock metrics (read-only from now on)

2. **Performance Report Generated**
   - Comprehensive PDF report created
   - All charts and graphs rendered
   - Comparison to goals calculated
   - Sent to both brand and creator

3. **Payment Release Triggered** (if escrow enabled)
   - Escrow system initiates release process
   - Brand has 48-72 hours to review before auto-release
   - Creator notified of pending payment

### Status Update
- **Status**: `Completed - Pending Payment`
- **Completed At**: Timestamp
- **Final Report**: Link to download

## Payment Processing

### Route
- **Primary**: `/app/tools/billing`
- **Campaign Specific**: `/app/tools/campaigns/:id/payment`

## Platform Payment (Escrow)

### Step 1: Review Performance
**Brand Dashboard:**
- View final metrics
- Compare to campaign goals
- Review deliverables checklist:
  - [ ] All content posted as agreed
  - [ ] Posted on time
  - [ ] Met minimum performance thresholds (if any)
  - [ ] Followed brand guidelines
  - [ ] FTC compliance maintained

### Step 2: Payment Decision

#### If Satisfactory (Most Common)
1. Click "Approve Payment" button
2. Confirm release of funds
3. **Automatic Actions:**
   - Funds released from escrow to creator account
   - Platform fee deducted (typically 10-15%)
   - Creator receives net amount
   - Receipt/invoice generated
   - Tax forms updated (1099 tracking if applicable)

**Breakdown Example:**
```
Campaign Budget: $2,000
Platform Fee (10%): -$200
Creator Receives: $1,800
Brand Total Paid: $2,000 (already charged)
```

4. Status → `Completed - Paid`

#### If Issues (Rare)
1. Click "Report Issue" button
2. Fill dispute form:
   - **Issue Type**: Dropdown
     - Content not posted
     - Posted late
     - Content removed
     - Brand guidelines violated
     - Performance significantly below expected
     - FTC compliance issues
     - Other
   - **Details**: Text area (required, min 100 chars)
   - **Evidence**: Upload screenshots, links, etc.
   - **Desired Outcome**: Full refund, partial refund, payment hold, etc.

3. **Dispute Resolution Process** initiates
4. Status → `Dispute - Under Review`

### Step 3: Dispute Resolution (If Applicable)

**Platform Mediation:**
1. **Case Assigned**: Platform support team reviews
2. **Evidence Review**: Both parties submit evidence
3. **Timeline**: 5-7 business days for resolution
4. **Possible Outcomes:**
   - **Full Payment**: No issue found, creator gets 100%
   - **Partial Payment**: Issue found, prorated payment (e.g., 70%)
   - **Full Refund**: Serious violation, brand gets refund
   - **Compromise**: Negotiated settlement

**Escalation:**
- If no agreement: Legal arbitration clause in contract
- Platform provides documentation for both parties

## External Payment (No Escrow)

### Manual Process
1. Brand marks payment as sent
2. Upload invoice/receipt (optional)
3. Select payment method used:
   - Bank transfer
   - PayPal
   - Wire transfer
   - Check
   - Crypto
   - Other

4. Creator confirms receipt:
   - Route: `/app/tools/campaigns/:id/confirm-payment`
   - Clicks "Confirm Payment Received"
   - Optional: Upload receipt

5. Status → `Completed - Paid`

### Payment Methods

**Credit/Debit Card (Platform):**
- Processed through Stripe integration
- Instant transfer (minus processing time)
- Platform fee included in transaction

**Bank Transfer:**
- ACH (US): 2-3 business days
- SEPA (EU): 1-2 business days
- Wire: Same day to 1 business day
- Fees vary by method

**PayPal:**
- Instant transfer to PayPal account
- Creator can withdraw to bank
- PayPal fees apply (~2.9% + $0.30)

**Wire Transfer:**
- International campaigns
- Higher fees but reliable
- 1-3 business days

**Cryptocurrency (if supported):**
- USDC, ETH, BTC supported
- Instant transfer
- Lower fees
- Requires wallet address verification

## Review & Rating

### Trigger
- After payment processed
- Email + in-app prompt: "Please review your experience with [Creator]"

### Rating Form

**Route**: `/app/tools/campaigns/:id/review`

#### Star Ratings (1-5 stars each)
1. **⭐ Overall Experience**: General satisfaction
2. **⭐ Communication**: Responsiveness, clarity, professionalism
3. **⭐ Content Quality**: Creativity, production value, alignment with brand
4. **⭐ Professionalism**: Reliability, adherence to guidelines
5. **⭐ Timeliness**: Met deadlines, posted on time

#### Written Review (Optional but Encouraged)
- **What went well**: Positive highlights (text area)
- **Areas for improvement**: Constructive feedback (text area)
- **Would you work together again?**: Yes/No/Maybe (radio buttons)

#### Additional Feedback
- **Tags**: Select all that apply
  - [ ] Great communicator
  - [ ] Creative
  - [ ] Professional
  - [ ] Met deadlines
  - [ ] Exceeded expectations
  - [ ] Good ROI
  - [ ] Would recommend

### Visibility
- **Creator Profile**: Review shows publicly on profile
- **Platform**: Helps future brands make decisions
- **Private Feedback**: Option to send private feedback to creator (not public)

### Creator Can Respond
- Creator can reply to review (once)
- Shows on profile below review
- Must be professional (monitored by platform)

### Impact on Creator
- **Rating Average**: Updates overall creator rating
- **Success Rate**: Affects success rate % (if campaign marked successful)
- **Top Rated Badge**: If maintains 4.8+ average, gets badge
- **Discovery Ranking**: Higher ratings = better visibility in search

## Database Models (Inferred)
- `Payment` (payment record)
  - campaignId
  - amount
  - platformFee
  - creatorNetAmount
  - paymentMethod
  - escrow (boolean)
  - processedAt
  - status (pending, approved, released, refunded, disputed)
  - transactionId (Stripe, PayPal, etc.)

- `Dispute` (dispute record)
  - campaignId
  - initiatedBy (brand/creator)
  - issueType
  - description
  - evidence (file URLs)
  - desiredOutcome
  - status (pending, investigating, resolved)
  - resolution
  - resolvedAt
  - resolvedBy (support staff ID)

- `Review` (rating record)
  - campaignId
  - reviewerId (brandId)
  - revieweeId (creatorId)
  - overallRating (1-5)
  - communicationRating (1-5)
  - contentQualityRating (1-5)
  - professionalismRating (1-5)
  - timelinessRating (1-5)
  - positiveComments (text)
  - improvementComments (text)
  - wouldWorkAgain (boolean)
  - tags (array)
  - isPublic (boolean)
  - creatorResponse (text, nullable)
  - createdAt

- `Invoice` (invoice record)
  - campaignId
  - invoiceNumber
  - amount
  - platformFee
  - tax (if applicable)
  - totalAmount
  - issuedAt
  - paidAt
  - fileUrl (PDF)

## API Endpoints (Inferred)
- `POST /api/campaigns/:id/complete` - Mark campaign as complete
- `GET /api/campaigns/:id/payment-status` - Check payment status
- `POST /api/payments/:id/approve` - Approve payment release
- `POST /api/payments/:id/dispute` - Initiate dispute
- `POST /api/payments/:id/mark-sent` - Mark external payment as sent
- `POST /api/payments/:id/confirm-receipt` - Confirm payment received
- `GET /api/payments/:id/invoice` - Generate invoice PDF
- `POST /api/reviews` - Submit review
- `GET /api/reviews/campaign/:id` - Get reviews for campaign
- `POST /api/reviews/:id/respond` - Creator responds to review
- `GET /api/disputes/:id` - Get dispute details
- `POST /api/disputes/:id/resolve` - Resolve dispute

## Notifications & Emails

### Brand Perspective
- "Campaign completed - final report ready"
- "Please approve payment release for [Creator]"
- "Payment will auto-release in 48 hours if no action taken"
- "Payment approved - [Creator] has been paid"
- "Please review your experience with [Creator]"

### Creator Perspective
- "Campaign completed - pending payment"
- "Brand is reviewing your work"
- "Good news! Payment approved and on the way"
- "Payment received: $1,800 (net of fees)"
- "[Brand] left you a review"

## Best Practices

✅ **DO:**
- Pay on time (or approve release promptly)
- Leave honest, constructive reviews
- Thank creators for good work (goes a long way)
- Save successful partnerships for future campaigns
- Analyze results to improve future campaigns
- Provide private feedback if you have concerns

❌ **DON'T:**
- Delay payment without reason
- Leave no feedback (hurts creator's ability to improve)
- Reuse content beyond agreed rights
- Forget to follow up for future campaigns
- Leave unfair negative reviews (be constructive)
- Dispute payment over minor issues

## Financial Tracking

### For Brand Accounting
- **Expense Category**: Marketing / Influencer Partnerships
- **Invoice**: Generated automatically, includes:
  - Campaign ID
  - Creator name
  - Content delivered
  - Performance metrics
  - Payment breakdown
- **Tax Documentation**: 1099 tracking for US creators (if payments > $600/year)

### For Creator Accounting
- **Income Category**: Freelance / Creator Income
- **Receipt**: Platform provides receipt with:
  - Net payment amount
  - Platform fee deducted
  - Tax ID (for filing)
- **Tax Forms**: Annual 1099 form (US) or similar based on region

## Edge Cases

### Auto-Release After Timeout
- If brand doesn't review within 72 hours
- Payment auto-released to creator
- Brand can still leave review after payment
- Protects creators from indefinite payment holds

### Payment Failed
- Credit card declined, bank transfer failed, etc.
- Brand notified to update payment method
- Creator notified of delay
- Payment retried after method updated
- Campaign remains `Pending Payment` until resolved

### Creator Account Suspended
- If creator violates platform terms after campaign
- Payment still released (for work completed)
- But future campaigns blocked
- Brand notified of suspension status

### Refund After Payment
- Very rare, requires serious violation
- Dispute must be opened within 30 days
- Evidence required (fraud, content removed, etc.)
- Platform reviews and may claw back payment
