# Phase 5: Content Creation

## 5.1 Review Brief & Guidelines

### Route
**Primary**: `/app/contracts/:contractId`

### Checklist Before Starting Work

**Pre-Production Checklist:**
- [ ] Read all deliverable requirements thoroughly
- [ ] Note required hashtags (#ad, #sponsored, brand hashtags)
- [ ] Note required @mentions (brand handles)
- [ ] Review do's and don'ts carefully
- [ ] Check content guidelines and tone requirements
- [ ] Review brand assets (logo, images, colors, fonts)
- [ ] Understand messaging requirements (key talking points)
- [ ] Confirm timeline and deadlines
- [ ] Download all brand assets (logo, images, guidelines PDF)
- [ ] Note FTC disclosure requirements placement
- [ ] Clarify any questions with brand BEFORE starting

### Key Information to Extract

**Deliverables:**
- Exact count (2 posts, 3 stories, 1 reel)
- Platform per deliverable
- Content type (static, video, carousel)
- Specifications (duration, dimensions)

**Messaging:**
- Key talking points (what must be mentioned)
- Tone (professional, casual, fun, inspiring)
- Do's (what to include)
- Don'ts (what to avoid)

**Technical Requirements:**
- Aspect ratio (1:1, 4:5, 9:16)
- Resolution (1080x1080, 1920x1080)
- Video length (15s, 30s, 60s)
- File format (MP4, JPG, PNG)

**Approval Process:**
- Pre-approval required or trust creator
- Approval deadline (24 hours, 48 hours)
- Number of revision rounds included

## 5.2 Use AI Tools for Content Creation

### AI Studio

**Route**: `/app/tools/ai-studio`

**Powered By**: OpenAI (GPT-4) / Claude (Anthropic)

**Capabilities:**
- **Caption Generation**: AI-powered caption writing
- **Script Writing**: Video script creation
- **Hook Generator**: Attention-grabbing openers
- **Hashtag Suggestions**: Relevant hashtag generation
- **Tone Refinement**: Match brand voice
- **Content Optimization**: Improve existing copy

**Example Usage:**
```
Prompt: "Write an Instagram caption for a skincare brand
launch. Product is vitamin C serum. Tone: Educational but fun.
Audience: Women 25-40. Include #ad disclosure."

AI Output:
"Glow season is HERE! ✨ I've been testing @BrandName's new
Vitamin C serum for 2 weeks and my skin is GLOWING. The
science is simple: vitamin C = collagen boost + brighter skin.
Try it yourself! Link in bio. #ad #skincare #vitaminC #glowup"
```

### Content Ideas API

**Endpoint**: `GET /api/v1/content-ideas`

**Features:**
- AI suggestions aligned with specific campaign
- Trending content formats in your niche
- Hook and angle recommendations
- Examples from successful creators

**Example Output:**
```
Content Ideas for Beauty Campaign:
1. "Get Ready With Me" using the product
2. Before/After transformation (2-week results)
3. "Why I switched to [Brand]" storytelling
4. Product comparison (this vs. competitor)
5. Ingredient breakdown (educational)
```

### Best Time to Post API

**Endpoint**: `GET /api/v1/best-time`

**Features:**
- Optimal posting time per platform
- Based on YOUR audience's activity patterns
- Maximizes initial engagement window
- Day of week recommendations

**Example Output:**
```
Instagram: Tuesday & Thursday, 6:00 PM EST
TikTok: Monday, Wednesday, Friday, 3:00 PM EST
YouTube: Sunday, 10:00 AM EST
```

## 5.3 Create Content

### Content Creation Tools

**Platform Routes:**
- **Post Creation**: `/app/create-post` - Text, image, video posts
- **Short-Form Video**: `/app/create/short` - Reels, Shorts, TikToks
- **Long-Form Video**: `/app/create/vlog` - YouTube videos, vlogs
- **Stories**: `/app/my-stories` - Ephemeral content

**Specialized Tools:**
- **Video Editor**: `/api/v1/video-editor` - Built-in editing tools
- **Live Streaming**: `/app/tools/stream` - Live content with chat and tips

### Content Quality Checklist

**Before Submission:**
- [ ] Follows brand guidelines strictly
- [ ] Includes all required hashtags (#ad, #sponsored, brand hashtags)
- [ ] Includes all required @mentions
- [ ] FTC disclosure visible without "see more" (#ad in first 3 hashtags)
- [ ] Maintains your authentic voice/style (don't sound robotic)
- [ ] Meets quality standards for your tier
  - Micro: Clean, clear content
  - Mid: Professional editing
  - Macro/Mega: Studio quality
- [ ] Link in bio set (if required)
- [ ] Correct aspect ratio for target platform
  - Instagram Post: 1:1 or 4:5
  - Instagram Reel: 9:16
  - TikTok: 9:16
  - YouTube: 16:9
- [ ] Resolution meets minimum (1080p minimum)
- [ ] Audio quality is clear (no background noise)
- [ ] Lighting is professional (well-lit, no harsh shadows)
- [ ] Captions/subtitles added (if video)

### Content Management

**Drafts System:**
- **API**: `/api/v1/drafts`
- Save work in progress
- Auto-save every 30 seconds
- Access drafts across devices

**Content Queue:**
- **API**: `/api/v1/queue`
- Schedule content for posting
- Set publish time and date
- Auto-post when time arrives

**Cross-Platform Publishing:**
- **API**: `/api/v1/cross-platform`
- Publish to multiple platforms simultaneously
- Supported: Instagram, YouTube, TikTok, Facebook, Twitter, LinkedIn
- Customize caption/format per platform

## 5.4 Communicate with Brand During Creation

### Route
**Primary**: `/app/messages`

### Communication Best Practices

**Proactive Updates:**
- **Progress milestones**: "Just finished shooting, editing next!"
- **Timeline updates**: "On track for Friday submission"
- **Challenges encountered**: "Product arrived damaged, can you send replacement?"
- **Questions**: "Should I focus more on X or Y feature?"

**When to Reach Out:**

**✅ Do reach out when:**
- You have questions about guidelines
- You need additional brand assets
- Timeline concerns arise (potential delay)
- Product issues (damaged, wrong item, not received)
- Creative direction clarification needed
- Unexpected obstacles

**❌ Don't reach out for:**
- Every minor detail (be self-sufficient)
- After already starting work on a major assumption
- To renegotiate terms mid-campaign

### Message Timing

**Best Practices:**
- **Ask questions early**: Don't wait until deadline to clarify
- **Update weekly**: Share progress once a week minimum
- **Respond within 24 hours**: To brand's messages
- **Share BTS content**: Builds brand relationship (optional)

### Status During Creation
**Status**: `In Progress - Creating Content`

## Database Models (Inferred)

### ContentDraft Model
```
ContentDraft {
  id: uuid
  contractId: foreign key
  milestoneId: foreign key
  creatorId: foreign key
  contentType: enum (POST, REEL, STORY, VIDEO, IGTV)
  platform: enum (INSTAGRAM, YOUTUBE, TIKTOK, etc.)
  caption: text
  hashtags: array
  mentions: array
  mediaUrls: array (draft files)
  status: enum (DRAFT, READY_TO_SUBMIT)
  autoSavedAt: timestamp
  createdAt: timestamp
  updatedAt: timestamp
}
```

### ContentQueue Model
```
ContentQueue {
  id: uuid
  creatorId: foreign key
  contractId: foreign key (nullable, for campaign content)
  platform: enum (INSTAGRAM, YOUTUBE, TIKTOK, etc.)
  contentType: enum
  caption: text
  hashtags: array
  mediaUrls: array
  scheduledFor: timestamp
  status: enum (SCHEDULED, PUBLISHED, FAILED)
  publishedAt: timestamp (nullable)
}
```

### ProgressUpdate Model
```
ProgressUpdate {
  id: uuid
  contractId: foreign key
  milestoneId: foreign key
  creatorId: foreign key
  updateType: enum (STARTED, IN_PROGRESS, BLOCKING_ISSUE, COMPLETED)
  message: text
  percentComplete: integer (0-100)
  createdAt: timestamp
}
```

## API Endpoints (Inferred)

### Content Creation
- `POST /api/v1/create-post` - Create new post
- `POST /api/v1/create/short` - Create short-form video
- `POST /api/v1/create/vlog` - Create long-form video
- `POST /api/v1/my-stories` - Create story

### Drafts
- `GET /api/v1/drafts` - Get all drafts
- `POST /api/v1/drafts` - Create draft
- `PUT /api/v1/drafts/:id` - Update draft
- `DELETE /api/v1/drafts/:id` - Delete draft
- `POST /api/v1/drafts/:id/auto-save` - Auto-save (called every 30s)

### Queue
- `GET /api/v1/queue` - Get scheduled content
- `POST /api/v1/queue` - Schedule content
- `PUT /api/v1/queue/:id` - Update scheduled content
- `DELETE /api/v1/queue/:id` - Cancel scheduled post
- `POST /api/v1/queue/:id/publish-now` - Publish immediately

### Cross-Platform
- `POST /api/v1/cross-platform/publish` - Publish to multiple platforms
- `GET /api/v1/cross-platform/status/:id` - Check publish status

### AI Tools
- `POST /api/v1/ai-studio/generate` - Generate content
- `GET /api/v1/content-ideas` - Get content suggestions
- `GET /api/v1/best-time` - Get optimal posting times

### Video Editor
- `POST /api/v1/video-editor/upload` - Upload footage
- `POST /api/v1/video-editor/trim` - Trim video
- `POST /api/v1/video-editor/merge` - Merge clips
- `POST /api/v1/video-editor/export` - Export final video

### Progress Updates
- `POST /api/v1/contracts/:id/progress` - Post progress update
- `GET /api/v1/contracts/:id/progress` - Get progress history

## Best Practices

### Content Quality
✅ **DO:**
- Follow brand guidelines precisely
- Maintain your authentic voice (don't sound scripted)
- Include ALL required elements (hashtags, mentions, disclosures)
- Use high-quality equipment (good camera, mic, lighting)
- Edit professionally (color correction, sound mixing)
- Create content you'd be proud to show in your portfolio
- Test posting time with Best Time to Post API

❌ **DON'T:**
- Ignore brand guidelines for "creative freedom"
- Sound like a commercial (maintain authenticity)
- Forget FTC disclosures (#ad must be visible)
- Use low-quality footage (blurry, dark, shaky)
- Rush production (quality over speed)
- Copy other creators' approach (be original)

### Communication
✅ **DO:**
- Ask clarifying questions BEFORE creating
- Share progress updates proactively
- Respond to brand within 24 hours
- Report timeline concerns immediately
- Request additional assets if needed
- Be professional and friendly

❌ **DON'T:**
- Assume unclear requirements (ask first)
- Ghost brand during creation phase
- Make excuses for delays
- Wait until deadline to report issues
- Be defensive about feedback
- Over-communicate minor details

### Time Management
✅ **DO:**
- Start work immediately after contract signed
- Build in buffer time for revisions
- Use drafts system to save work
- Schedule posts using content queue
- Plan shoots around optimal lighting
- Leave time for editing and polishing

❌ **DON'T:**
- Procrastinate until last minute
- Assume you'll nail it in one take
- Skip planning and scripting
- Rush editing to meet deadline
- Forget to save work (use auto-save)
- Miss submission deadlines

### Using AI Tools
✅ **DO:**
- Use AI for inspiration and first drafts
- Customize AI-generated content to match your voice
- Use AI to refine and optimize captions
- Let AI handle hashtag research
- Use AI for scripting structure

❌ **DON'T:**
- Copy AI-generated content verbatim (sounds generic)
- Rely 100% on AI (loses authenticity)
- Forget to add personal touch
- Use AI hashtags without verifying relevance
- Let AI replace your creativity (AI assists, doesn't replace)
