# Phase 3: Campaign Creation & Setup

## Routes
- **Primary**: `/app/tools/campaigns` - Campaign manager
- **Create**: Opens create panel/modal
- **Entry from**: "Hire Creator" button on profile

## 8-Step Campaign Creation Form

### Step 1: Basic Information
**Fields:**
- **Campaign Name**: Internal reference (e.g., "Spring Product Launch")
- **Campaign Type**: Dropdown
  - Sponsored Post
  - Product Review
  - Brand Ambassador
  - Event Coverage
  - Unboxing
  - Tutorial/How-to
  - Giveaway/Contest
- **Campaign Goal**: Dropdown
  - Brand Awareness
  - Product Launch
  - Lead Generation
  - Sales/Conversions
  - App Downloads
  - Event Promotion

### Step 2: Creator Selection
**Fields:**
- **Pre-selected Creator**: Auto-filled if from profile
- **Add More Creators**: Multi-select for multi-creator campaigns
- **Backup Creators**: Alternates if primary declines

### Step 3: Deliverables
**Platform Selection:**
- Instagram, YouTube, TikTok, Twitter, Facebook, LinkedIn

**Content Types (checkboxes with quantities):**
- [ ] Feed Posts (qty: ___)
- [ ] Stories (qty: ___)
- [ ] Reels/Shorts (qty: ___)
- [ ] Videos (qty: ___, duration: ___)
- [ ] IGTV/Long-form (qty: ___)

**Content Requirements (checkboxes):**
- [ ] Must include product/brand mention
- [ ] Specific hashtags to use (list)
- [ ] @mentions required (list)
- [ ] Link in bio
- [ ] Swipe-up links (if available)

**Creative Freedom (radio buttons):**
- [ ] Strict brand guidelines (detailed approval)
- [ ] Moderate flexibility (general guidelines)
- [ ] Full creative control (trust creator)

### Step 4: Content Guidelines
**Brand Assets (file uploads):**
- Upload logo
- Upload product images
- Upload brand guidelines PDF
- Color palette (hex codes)
- Fonts (list)

**Messaging (text areas):**
- Key talking points (bullet list)
- Do's and Don'ts
- Prohibited content (list)
- Disclosure requirements (#ad, #sponsored, etc.)

**Approval Process (radio buttons):**
- [ ] Require pre-approval before posting
- [ ] Trust creator (no approval needed)
- [ ] Approval within X hours (specify)

### Step 5: Timeline
**Date Fields:**
- **Start Date**: When campaign begins
- **Content Submission Deadline**: When creator submits for review
- **Publishing Date**: When content goes live
- **Campaign End Date**: When tracking period ends

### Step 6: Budget & Payment
**Budget:**
- **Amount**: $_____ (auto-filled from creator's pricing if selected)
- **Currency**: USD, EUR, etc.

**Payment Terms (radio buttons):**
- [ ] Upfront (50% before, 50% after)
- [ ] Upon completion
- [ ] Net 30/60/90
- [ ] Milestone-based (specify milestones)

**Usage Rights:**
- **Duration**: Dropdown (30 days, 90 days, 1 year, perpetual)
- **Platforms**: Checkboxes (Instagram only, all social, + website, + paid ads)
- **Exclusivity Clause**: Checkbox + duration (can't work with competitors for X months)

### Step 7: Legal & Contracts
**Options (checkboxes):**
- [ ] Use platform standard contract
- [ ] Upload custom contract (file upload)
- [ ] Non-disclosure agreement (NDA)
- [ ] FTC compliance acknowledgment

### Step 8: Review & Submit
**Summary Display:**
- Campaign overview
- Estimated reach (calculated)
- Estimated engagement (calculated)
- Budget breakdown (platform fee, creator payment, total)
- Timeline visualization (Gantt chart)

**Actions:**
- 💾 **Save as Draft**: Save without sending
- 👥 **Send for Team Review**: Internal approval flow
- ✉️ **Send Campaign Invite to Creator**: Final submission

## Campaign Invitation Sent

### Creator Receives
- **Email notification**: Campaign invite with summary
- **In-app notification**: Red badge, push notification
- **Campaign details page**: `/app/campaigns/invites/:id`

### Creator Actions
- ✅ **Accept campaign**: Proceed to Phase 4
- ❌ **Decline campaign**: Reject with optional reason
- 💬 **Negotiate terms**: Counter-offer (→ Phase 4)
- ❓ **Ask questions**: Message brand before deciding

### Status Update
- **Initial Status**: `Pending Acceptance`
- **Notification to Brand**: "Campaign invite sent to [Creator]"

## Database Models (Inferred)
- `Campaign` (main campaign record)
  - name, type, goal, status
  - startDate, submissionDeadline, publishDate, endDate
  - budget, paymentTerms, usageRights
- `CampaignCreator` (many-to-many junction)
  - campaignId, creatorId, role (primary/backup), status
- `CampaignDeliverable` (content requirements)
  - platform, contentType, quantity, duration
- `CampaignGuideline` (brand guidelines)
  - assets (JSON), messaging, approvalProcess
- `CampaignContract` (legal documents)
  - type, fileUrl, signed, signedAt
- `CampaignInvitation` (invite tracking)
  - sentAt, respondedAt, status, response
- `CampaignMilestone` (milestone-based payments)
  - description, amount, dueDate, completed

## API Endpoints (Inferred)
- `POST /api/campaigns` - Create new campaign
- `PUT /api/campaigns/:id` - Update draft campaign
- `POST /api/campaigns/:id/send-invite` - Send invitation
- `GET /api/campaigns/:id` - Get campaign details
- `POST /api/campaigns/:id/team-review` - Request internal review
- `GET /api/campaigns/drafts` - Get saved drafts
- `GET /api/creators/:id/estimated-reach` - Calculate reach
- `POST /api/campaigns/:id/assets` - Upload brand assets

## Best Practices
✅ **DO:**
- Provide comprehensive brand guidelines upfront
- Set realistic timelines (allow buffer for revisions)
- Allow creative freedom within guidelines
- Include all necessary assets at creation
- Clarify approval process expectations clearly
- Budget should include platform fees (10-15%)

❌ **DON'T:**
- Micromanage every detail
- Set impossible deadlines (e.g., 24hr turnaround)
- Change requirements mid-campaign
- Withhold important information
- Skip the contract/legal step
- Forget to specify usage rights clearly

## Validation Rules
- Campaign name: required, 3-100 chars
- At least 1 creator selected
- At least 1 deliverable specified
- Budget > $0
- Publishing date > submission deadline > start date
- Contract required for budgets > $5000
