# CuraLink Integration Guide

## MongoDB Setup

### Connection String
\`\`\`
MONGODB_URI=mongodb+srv://username:password@curalink.mongodb.net/curalink_db?retryWrites=true&w=majority
\`\`\`

### Required Databases & Collections
- **Database**: curalink_db
- **Collections**: users, clinical_trials, messages, voice_logs, sms_logs, forums

## Message Agent (Twilio SMS)

### Setup Steps
1. Sign up at https://www.twilio.com
2. Get your Account SID and Auth Token
3. Add environment variables:
   \`\`\`
   TWILIO_ACCOUNT_SID=your_account_sid
   TWILIO_AUTH_TOKEN=your_auth_token
   TWILIO_PHONE_NUMBER=+1234567890
   \`\`\`

### Usage Example
\`\`\`typescript
import twilio from 'twilio';

const client = twilio(
  process.env.TWILIO_ACCOUNT_SID,
  process.env.TWILIO_AUTH_TOKEN
);

// Send appointment reminder
await client.messages.create({
  body: 'Reminder: Your clinical trial appointment is tomorrow at 2 PM',
  from: process.env.TWILIO_PHONE_NUMBER,
  to: '+1patient_number'
});
\`\`\`

## Voice Agent (Twilio Voice)

### Setup Steps
1. Use same Twilio account
2. Create TwiML Webhook for voice handling
3. Connect to OpenAI/Claude for AI responses

### Voice Agent Architecture
\`\`\`
Patient Calls → Twilio Voice API 
  → Node.js Route Handler 
  → AI SDK (for understanding) 
  → MongoDB (for patient data)
  → Response via Twilio TTS (Text-to-Speech)
\`\`\`

### Example Route Handler
\`\`\`typescript
// app/api/voice/ivr/route.ts
import { NextRequest, NextResponse } from 'next/server';
import twilio from 'twilio';

export async function POST(request: NextRequest) {
  const twiml = new twilio.twiml.VoiceResponse();
  
  twiml.gather({
    numDigits: 1,
    action: '/api/voice/process',
    method: 'POST',
  }).say('Press 1 for appointment reminders, 2 for trial information');
  
  return new NextResponse(twiml.toString(), {
    headers: { 'Content-Type': 'application/xml' }
  });
}
\`\`\`

## AI Integration (Vercel AI SDK)

### Setup
\`\`\`bash
npm install ai @ai-sdk/openai
\`\`\`

### Usage for AI Summaries & Recommendations
\`\`\`typescript
import { generateText } from 'ai';
import { openai } from '@ai-sdk/openai';

const { text } = await generateText({
  model: openai('gpt-4'),
  prompt: `Summarize this clinical trial: ${trialDescription}`,
});
\`\`\`

## Environment Variables Needed

\`\`\`
# MongoDB
MONGODB_URI=

# Twilio (SMS)
TWILIO_ACCOUNT_SID=
TWILIO_AUTH_TOKEN=
TWILIO_PHONE_NUMBER=

# Twilio (Voice)
TWILIO_VOICE_URL=

# AI (OpenAI)
OPENAI_API_KEY=

# Authentication
JWT_SECRET=

# Application
NEXT_PUBLIC_APP_URL=http://localhost:3000
\`\`\`

## Deployment Notes

1. Deploy to Vercel for best compatibility with Next.js
2. Set all environment variables in Vercel project settings
3. Twilio webhooks should point to your deployed URL
4. MongoDB Atlas allows connections from Vercel IP ranges
