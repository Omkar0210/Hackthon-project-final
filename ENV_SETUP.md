# CuraLink Environment Variables Setup

## Required Variables

### MongoDB
\`\`\`
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/curalink?retryWrites=true&w=majority
\`\`\`

Get this from MongoDB Atlas:
1. Go to https://www.mongodb.com/cloud/atlas
2. Create a cluster
3. Go to "Database" → "Connect" → "Drivers"
4. Copy the connection string
5. Replace `<password>` with your database password

### VAPI Voice Agent
\`\`\`
NEXT_PUBLIC_VAPI_API_KEY=fa189611-80e1-470c-a3d8-0a941852a956
NEXT_PUBLIC_VAPI_ASSISTANT_ID=350e5c66-88a2-493d-9958-a2b955ad94de
\`\`\`

These are already provided and configured.

### Twilio SMS
\`\`\`
TWILIO_ACCOUNT_SID=your_account_sid
TWILIO_AUTH_TOKEN=your_auth_token
TWILIO_PHONE_NUMBER=+1234567890
\`\`\`

Get these from Twilio:
1. Go to https://www.twilio.com
2. Sign up for a free account
3. Go to Console dashboard
4. Find "Account SID" and "Auth Token"
5. Buy a phone number and add it as TWILIO_PHONE_NUMBER

### OpenAI (for AI summaries)
\`\`\`
OPENAI_API_KEY=sk-...
\`\`\`

Get this from https://platform.openai.com/api-keys

## How to Add Environment Variables in Vercel

1. Go to your Vercel dashboard
2. Select your project
3. Click "Settings" → "Environment Variables"
4. Add each variable (Name and Value)
5. Click "Save"
6. Redeploy your project for changes to take effect

## Development Setup (.env.local)

For local development, create a `.env.local` file:

\`\`\`
MONGODB_URI=mongodb+srv://...
NEXT_PUBLIC_VAPI_API_KEY=fa189611-80e1-470c-a3d8-0a941852a956
NEXT_PUBLIC_VAPI_ASSISTANT_ID=350e5c66-88a2-493d-9958-a2b955ad94de
TWILIO_ACCOUNT_SID=...
TWILIO_AUTH_TOKEN=...
TWILIO_PHONE_NUMBER=+1...
OPENAI_API_KEY=sk-...
\`\`\`

**Important**: Never commit `.env.local` to Git!

## Vercel Integration

When deploying to Vercel:
1. Add all environment variables in the Vercel dashboard
2. They will automatically be available in your deployed application
3. `NEXT_PUBLIC_*` variables are exposed to the browser
4. Other variables are only available on the server
