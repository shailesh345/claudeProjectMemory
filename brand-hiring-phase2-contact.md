# Phase 2: Initial Contact & Outreach

## Routes
- **Primary**: `/app/messages` - Centralized messaging hub
- **Entry from**: Profile page "Message" button

## Messaging Flow

### Initiation
1. Click "Message" button on creator profile
2. Auto-navigates to `/app/messages` with creator pre-selected
3. Composer opens with creator context

### Message Composition
**Include:**
- Introduction (who you are, your brand)
- Campaign overview (what you're promoting)
- Budget range (transparency)
- Timeline (when content needed)
- Call to action (next steps)

### Message Templates
- **"Collaboration Inquiry"**: General interest template
- **"Campaign Brief"**: Detailed campaign pitch
- **"Quick Partnership Pitch"**: Short, direct approach

## Features

### Communication Tools
- **Attach Files**: Briefs, mood boards, brand guidelines
- **Real-time Chat**: Live messaging
- **Read Receipts**: Know when creator viewed message
- **Typing Indicators**: See when creator is responding
- **Video Call Option**: For detailed discussions (scheduling)

### Message Thread Management
- Conversation history
- Creator profile sidebar
- Quick actions (view profile, hire, block)
- Message search
- Archive/delete conversations

## Media Kit Request

### Action
- Click "Download Media Kit" button on profile
- PDF generated and downloaded

### Media Kit Contents
1. **Professional bio**: Creator background and expertise
2. **Platform statistics**: Detailed metrics across all platforms
3. **Audience demographics**: Age, gender, location, interests (detailed)
4. **Past brand collaborations**: Portfolio with results
5. **Content examples**: Best-performing posts
6. **Rate card**: Pricing for different deliverables
7. **Contact information**: Email, social handles
8. **Testimonials**: Quotes from past brand partners

## Database Models (Inferred)
- `Conversation` (brand-creator thread)
- `Message` (individual messages)
- `MessageAttachment` (file uploads)
- `MediaKit` (PDF metadata, generation log)
- `MessageTemplate` (reusable templates)
- `VideoCall` (scheduling, recordings)

## API Endpoints (Inferred)
- `GET /api/messages` - Get all conversations
- `GET /api/messages/:conversationId` - Get message thread
- `POST /api/messages` - Send new message
- `POST /api/messages/:id/read` - Mark as read
- `POST /api/messages/attachments` - Upload file
- `GET /api/creators/:id/media-kit` - Generate/download PDF
- `POST /api/video-calls/schedule` - Schedule video call
- `GET /api/message-templates` - Get template list

## Real-time Events (WebSocket)
- `message.received` - New message notification
- `message.read` - Read receipt
- `typing.start` - Creator is typing
- `typing.stop` - Creator stopped typing
- `presence.online` - Creator online status
- `presence.offline` - Creator went offline

## Best Practices
✅ **DO:**
- Personalize each message (mention creator's work)
- Clearly state brand and campaign goal
- Be transparent about budget upfront
- Respect creator's creative input
- Respond promptly to questions (within 24 hours)

❌ **DON'T:**
- Send generic copy-paste messages
- Be vague about expectations
- Lowball pricing
- Demand unrealistic deliverables
- Ghost creators after initial contact
- Spam multiple messages if no response

## Communication Tips
- **Subject Line** (if supported): Clear, specific, personalized
- **Opening**: Reference specific work of theirs you admire
- **Body**: Brief campaign overview, why they're a fit
- **Budget**: Give range, not exact (allows negotiation)
- **Timeline**: Realistic, with flexibility
- **CTA**: Clear next step (e.g., "Available for a quick call?")
