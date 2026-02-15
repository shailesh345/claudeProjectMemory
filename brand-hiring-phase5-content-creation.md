# Phase 5: Content Creation & Collaboration

## Status Flow
```
Active - Awaiting Content → In Progress → Under Review → Approved - Ready to Publish
                                        ↘ Revision Requested → (back to In Progress)
                                        ↘ Rejected
```

## Creator Works on Content

### Brand Monitoring

**Route**: `/app/tools/campaigns/:id`

**Brand Can:**
1. **Monitor Progress** (if creator updates milestones)
   - Milestone checkboxes: "Script written", "Footage captured", "Editing", etc.
   - Progress bar (0-100%)
   - Last updated timestamp

2. **Answer Questions via Campaign Chat**
   - Integrated chat in campaign dashboard
   - Quick responses to creator questions
   - Share additional assets or clarifications

3. **Provide Additional Assets/Info**
   - Upload more brand materials if requested
   - Send product samples tracking info
   - Clarify messaging or guidelines

4. **Send Reminders for Approaching Deadlines**
   - Auto-reminders at: 7 days, 3 days, 1 day before deadline
   - Manual reminder button available
   - Gentle nudges, not pushy

### Status During Creation
- **Status**: `In Progress`
- **Visible**: Progress updates in campaign dashboard
- **Timeline**: Days remaining counter

### Notifications to Brand
- "Creator has started working on content" (when creator marks as started)
- "Creator has questions" (when message sent)
- "X days until submission deadline" (automated reminders)
- "Creator updated progress to 50%" (milestone updates)

## Content Submission for Approval

### Submission Flow

**Creator Action:**
1. Navigate to campaign in creator dashboard
2. Click "Submit Content for Approval"
3. Upload content files (images, videos)
4. Fill submission form:
   - Platform dropdown
   - Content type (Post, Story, Reel, etc.)
   - Caption/script (text area)
   - Hashtags (comma-separated or tags)
   - @mentions (list)
   - Links (URL fields)
   - Scheduled posting date/time
5. Click "Submit for Review"

**Brand Notification:**
- Email: "Content ready for review"
- In-app: Red badge with notification
- Push notification (mobile)

### Content Review Interface

**Route**: `/app/tools/campaigns/:id/review`

**Left Panel: Content Preview**
- **Visual Preview**: Image/video as it will appear
- **Platform Context**: Mockup showing how it looks on Instagram/TikTok/YouTube
- **Zoom/Fullscreen**: View in detail

**Right Panel: Metadata**
- **Platform**: Which platform (icon + name)
- **Content Type**: Post, Story, Reel, Video, etc.
- **Caption/Script**: Full text with character count
- **Hashtags**: List of tags (clickable)
- **Mentions**: @brand handles (clickable)
- **Links**: URL tracking links (test button)
- **Posting Date/Time**: Scheduled time + timezone
- **Duration**: Video length (for videos)
- **Aspect Ratio**: Dimensions check

**Bottom: Action Buttons**
- ✅ **Approve Content** (green button)
- 🔄 **Request Revision** (yellow button)
- ❌ **Reject Content** (red button - rare)

## Brand Actions

### ✅ Approve Content

**Flow:**
1. Click "Approve" button
2. Optional modal:
   - **Add Comment/Praise**: "Looks amazing! Love the creativity."
   - **Attach note**: Any final reminders
3. Confirm approval

**Result:**
- Creator gets approval notification (email + in-app)
- Status → `Approved - Ready to Publish`
- Creator can now schedule posting
- Brand dashboard shows "Approved ✓"

**Notification:**
- "Great news! Your content has been approved 🎉"
- Includes brand's comment if added

### 🔄 Request Changes

**Flow:**
1. Click "Request Revision"
2. Modal opens with form:

**Change Categories (checkboxes):**
- [ ] Caption needs adjustment
- [ ] Visual/editing changes needed
- [ ] Hashtags/mentions missing or incorrect
- [ ] Brand guidelines not followed
- [ ] Posting time/date needs change
- [ ] Other (explain)

**Detailed Feedback (text area):**
- Specific, actionable feedback
- Examples: "Please add #ad disclosure", "Can you use the blue product photo instead?"
- Tone: constructive, friendly, clear

**Priority (optional):**
- Minor: Nice to have
- Moderate: Important but not blocking
- Critical: Must fix before approval

3. Click "Send Revision Request"

**Result:**
- Creator gets detailed feedback
- Status → `Revision Requested`
- Creator makes changes and resubmits
- Revision counter increments (1/3, 2/3, 3/3)

**Notification to Creator:**
- "Brand requested changes to your content"
- Shows detailed feedback
- Link to revision guidelines

### ❌ Reject Content

**When to Use:**
- Serious brand safety issues
- Content violates campaign terms
- Inappropriate or offensive content
- Completely off-brief (rare)

**Flow:**
1. Click "Reject" button
2. Modal requires explanation:
   - **Reason**: Dropdown (brand safety, off-brief, inappropriate, other)
   - **Detailed Explanation**: Required text (min 50 chars)
   - **Next Steps**: What happens now (re-do, cancel campaign, etc.)
3. Confirm rejection

**Result:**
- Status → `Rejected`
- Triggers dispute resolution flow
- Platform team may get involved
- Campaign may be cancelled with partial refund

**Consequences:**
- Rare action, serious implications
- May affect creator's rating
- May trigger contract review

## Revision Process

### Flow
1. Creator receives revision request
2. Creator makes changes
3. Creator resubmits (same interface)
4. Brand reviews again
5. Repeat as needed

### Revision Limits
- **Typical**: 2-3 rounds max
- **Contract may specify**: e.g., "Up to 3 revisions included"
- **After limit**: Additional revisions may incur fees or require renegotiation

### Status During Revisions
- **Status**: `Revision Requested` → back to `In Progress` (when creator starts) → `Under Review` (when resubmitted)
- **Revision Counter**: Visible in dashboard (Round 1, Round 2, etc.)
- **History**: All previous submissions and feedback saved

## Database Models (Inferred)
- `ContentSubmission` (submission record)
  - campaignId
  - creatorId
  - submittedAt
  - platform
  - contentType
  - fileUrls (array of media files)
  - caption
  - hashtags (array)
  - mentions (array)
  - links (array)
  - scheduledDate, scheduledTime
  - revisionNumber (1, 2, 3...)
  - status (pending_review, approved, revision_requested, rejected)

- `ContentReview` (review record)
  - submissionId
  - reviewedBy (brandId)
  - reviewedAt
  - action (approve, request_revision, reject)
  - feedback (text)
  - changeCategories (array)
  - priority (minor, moderate, critical)

- `CampaignProgress` (milestones)
  - campaignId
  - milestones (JSON: [{name, completed, completedAt}])
  - percentComplete
  - lastUpdated

## API Endpoints (Inferred)
- `POST /api/campaigns/:id/submit-content` - Submit content for review
- `POST /api/campaigns/:id/content/upload` - Upload media files
- `GET /api/campaigns/:id/submissions` - Get all submissions
- `GET /api/submissions/:id` - Get specific submission
- `POST /api/submissions/:id/approve` - Approve content
- `POST /api/submissions/:id/request-revision` - Request changes
- `POST /api/submissions/:id/reject` - Reject content
- `PUT /api/campaigns/:id/progress` - Update progress milestones
- `GET /api/submissions/:id/history` - Get revision history

## Notifications & Emails

### Creator Perspective
- "Your content has been approved!" ✅
- "Brand requested changes to your content" 🔄
- "Content has been rejected" ❌ (rare)
- Deadline reminders: "3 days until submission deadline"

### Brand Perspective
- "Content ready for review from [Creator]"
- "Creator resubmitted content after revisions"
- "Reminder: Content pending your review"

## Best Practices

✅ **DO:**
- Review content promptly (within 24-48 hours)
- Provide specific, actionable feedback
- Approve good work even if not 100% perfect (80/20 rule)
- Be respectful in revision requests
- Communicate clearly with examples
- Praise what works before suggesting changes

❌ **DON'T:**
- Delay feedback for days
- Give vague feedback like "make it better" or "needs work"
- Demand complete content overhauls (start from scratch)
- Be rude or condescending
- Request unlimited revisions beyond contract terms
- Reject content over minor issues (use revision instead)

## Feedback Examples

**Good Feedback:**
- ✅ "Love the energy! Can you add #ad to the caption for FTC compliance?"
- ✅ "Great video! The blue product photo would match our brand better - can you swap the red one?"
- ✅ "Perfect caption, but could you @mention our Instagram handle @ourbrand?"

**Bad Feedback:**
- ❌ "Needs to be better"
- ❌ "Not what I wanted"
- ❌ "Change everything"
- ❌ "I don't like it" (no specifics)

## Edge Cases

### Creator Misses Deadline
- Auto-reminder sent
- Brand can extend deadline (1-time grace)
- After grace period: campaign marked `At Risk`
- May trigger penalty or cancellation

### Brand Delays Review
- Auto-reminders to brand after 48 hours
- After 7 days: Creator can escalate to support
- Platform may auto-approve if brand unresponsive for 14+ days

### Infinite Revision Loop
- Platform monitors revision count
- After 4+ rounds: suggests mediation or compromise
- Platform team may review both sides and provide binding decision
