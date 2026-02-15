# Phase 4: Negotiation & Agreement

## Status Flow
```
Pending Acceptance → In Negotiation → Active - Awaiting Content
                  ↘ Declined ↙
```

## Negotiation Process (Optional)

### When Creator Wants to Negotiate

**Negotiation Points:**
1. **Budget Adjustment**: Higher payment request
2. **Deliverables Modification**: Reduce quantity or change content types
3. **Timeline Changes**: More time needed for content creation
4. **Usage Rights Terms**: Shorter duration or limited platforms
5. **Payment Terms**: Different payment schedule

### Negotiation Flow

**Step 1: Creator Sends Counter-Offer**
- Navigate to invitation in creator dashboard
- Click "Negotiate Terms"
- Fill counter-offer form:
  - New budget amount (with justification)
  - Modified deliverables (with explanation)
  - Timeline adjustment request
  - Usage rights modification
  - Additional notes/requests

**Step 2: Brand Reviews Counter-Offer**
- Route: `/app/tools/campaigns/:id/negotiation`
- Notification: "Creator has sent a counter-offer"
- See side-by-side comparison:
  - Original offer (left column)
  - Creator's counter-offer (right column)
  - Differences highlighted in yellow

**Step 3: Brand Decision**
- **Accept Counter-Offer**:
  - Click "Accept"
  - Campaign updated with new terms
  - Status → `Active - Awaiting Content`
  - Contract regenerated if needed

- **Decline and Keep Original**:
  - Click "Keep Original Terms"
  - Notification sent to creator
  - Status remains `Pending Acceptance`
  - Ball back in creator's court

- **Send New Counter-Offer**:
  - Click "Send Counter-Offer"
  - Modify terms again
  - Add explanation/justification
  - Status stays `In Negotiation`

**Step 4: Back-and-Forth**
- Can go multiple rounds (typically 2-4 max)
- Each party can see negotiation history
- All changes tracked and logged
- Eventually reaches agreement or rejection

### Status During Negotiation
- **Status**: `In Negotiation`
- **Visible in**: Campaign list with yellow badge
- **Notifications**: Both parties get updates on each counter-offer

## Campaign Acceptance

### When Creator Accepts (No Negotiation)

**Route**: Creator clicks "Accept" on invitation

**Automatic Actions:**
1. **Contract Generation** (if using platform contract)
   - PDF generated with all campaign terms
   - Sent to both brand and creator

2. **E-Signatures Requested**
   - Brand receives email: "Sign campaign contract"
   - Creator receives email: "Sign campaign contract"
   - Integration with DocuSign or similar
   - Route: `/app/contracts/:id/sign`

3. **Payment Hold Initiated** (if escrow enabled)
   - Brand charged or authorized for campaign amount + platform fee
   - Funds held in escrow
   - Released upon campaign completion
   - Route: `/app/tools/billing/escrow/:id`

4. **Campaign Dashboard Activated**
   - Full campaign dashboard unlocked
   - Route: `/app/tools/campaigns/:id`
   - Collaboration features enabled

5. **Calendar Invites Sent**
   - Email calendar invites (.ics files)
   - For all key deadlines:
     - Content submission deadline
     - Publishing date
     - Campaign end date
   - Google Calendar / Outlook integration

### Status Update
- **New Status**: `Active - Awaiting Content`
- **Campaign Dashboard**: Now fully accessible at `/app/tools/campaigns/:id`

### Brand Dashboard View (`/app/tools/campaigns/:id`)

**Header:**
- Campaign name
- Creator profile (avatar, name, handle)
- Status badge
- Days until next deadline

**Tabs:**
1. **Overview**: Campaign summary, timeline, budget
2. **Deliverables**: What's required, submission status
3. **Guidelines**: Brand assets, messaging, requirements
4. **Messages**: Direct chat with creator
5. **Timeline**: Visual timeline with milestones
6. **Contract**: View/download signed contracts

**Quick Actions:**
- 💬 Message creator
- 📄 View contract
- 📊 See analytics (once live)
- ⚙️ Edit campaign (limited fields)
- 🚫 Cancel campaign (with reason)

## Database Models (Inferred)
- `CampaignNegotiation` (negotiation thread)
  - campaignId
  - roundNumber (1, 2, 3...)
  - initiatedBy (creator/brand)
  - originalTerms (JSON)
  - proposedTerms (JSON)
  - changes (JSON diff)
  - justification (text)
  - status (pending, accepted, rejected)
  - createdAt, respondedAt

- `CampaignContract` (extended fields)
  - contractId
  - campaignId
  - type (platform/custom)
  - fileUrl (PDF)
  - brandSigned (boolean)
  - brandSignedAt (timestamp)
  - brandSignature (image/encrypted)
  - creatorSigned (boolean)
  - creatorSignedAt (timestamp)
  - creatorSignature (image/encrypted)
  - fullyExecuted (boolean)

- `EscrowTransaction` (payment hold)
  - campaignId
  - amount
  - platformFee
  - totalAmount
  - authorizedAt
  - heldUntil
  - releasedAt
  - status (pending, held, released, refunded)

## API Endpoints (Inferred)
- `POST /api/campaigns/:id/accept` - Accept invitation
- `POST /api/campaigns/:id/decline` - Decline invitation
- `POST /api/campaigns/:id/negotiate` - Send counter-offer
- `GET /api/campaigns/:id/negotiations` - Get negotiation history
- `POST /api/campaigns/:id/negotiations/:negotiationId/respond` - Respond to counter-offer
- `POST /api/contracts/generate` - Generate contract PDF
- `POST /api/contracts/:id/sign` - E-sign contract
- `GET /api/contracts/:id` - Download contract
- `POST /api/escrow/authorize` - Authorize payment hold
- `GET /api/escrow/:campaignId/status` - Check escrow status
- `POST /api/calendar/send-invites` - Send calendar invites

## Notifications & Emails

### Creator Perspective
- "Your campaign invitation is pending" (daily reminder)
- "Brand has responded to your counter-offer"
- "Please sign the campaign contract"
- "Campaign is now active - time to create content!"

### Brand Perspective
- "Creator accepted your campaign invitation"
- "Creator sent a counter-offer"
- "Creator signed the contract - awaiting your signature"
- "Campaign is now active"

## Best Practices

✅ **DO:**
- Respond to counter-offers within 48 hours
- Be open to reasonable adjustments
- Justify your counter-offers clearly
- Keep negotiations professional and friendly
- Know your hard limits (budget cap, timeline constraints)

❌ **DON'T:**
- Drag negotiations beyond 3-4 rounds
- Make unreasonable demands
- Change terms drastically in negotiation
- Use negotiation to lowball
- Forget to factor in platform fees

## Edge Cases

### Creator Declines
- Status → `Declined`
- Brand receives notification with optional reason
- Can send invite to backup creator
- Original creator's invite archived

### Negotiation Stalemate
- After 3+ rounds, platform suggests mediation
- Both parties can request platform team help
- Typically resolved through compromise
- If no agreement: campaign cancelled, no fees

### Contract Signing Timeout
- If not signed within 7 days, reminder emails sent
- After 14 days, campaign marked `At Risk`
- After 30 days, auto-cancelled with refund
