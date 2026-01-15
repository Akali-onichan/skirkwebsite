# Discord Contact Form Integration Guide

This guide explains how to set up the contact form to send messages directly to your Discord.

## Setup Options

### Option 1: Discord Webhook (Recommended - Simpler)

**Why Use Webhooks?**
- Easier to set up
- No bot permissions required
- More reliable
- Can't be rate-limited like bot API

**Steps:**

1. **Create a Webhook in Your Discord Server**
   - Go to your Discord server
   - Click "Server Settings" (server name dropdown)
   - Go to "Integrations" → "Webhooks"
   - Click "New Webhook"
   - Choose the channel where you want to receive messages
   - Copy the Webhook URL

2. **Add the Webhook URL to Environment Variables**

   Create a `.env.local` file in your project root:

   ```bash
   DISCORD_WEBHOOK_URL=https://discord.com/api/webhooks/YOUR_WEBHOOK_ID/YOUR_WEBHOOK_TOKEN
   ```

3. **Restart the Development Server**

   ```bash
   bun run dev
   ```

That's it! The contact form will now send messages to your Discord channel with:
- User's name
- User's email
- Their message
- Timestamp
- Beautiful embed formatting

### Option 2: Discord Bot API

**Why Use Bot API?**
- More control over message formatting
- Can reply to messages programmatically
- Useful for future features

**Steps:**

1. **Create a Discord Bot Application**

   - Go to https://discord.com/developers/applications
   - Click "New Application"
   - Give it a name (e.g., "Skirk Bot Website")
   - Click "Create"

2. **Create a Bot User**

   - Go to the "Bot" tab
   - Click "Add Bot"
   - Set a bot name
   - Enable "Public Bot" (optional)
   - Enable "Message Content Intent" if needed
   - Save and copy the bot token

3. **Invite Bot to Your Server**

   - Go to "OAuth2" → "URL Generator"
   - Select scopes:
     - `bot`
   - Select bot permissions:
     - `Send Messages`
     - `Embed Links`
     - `Attach Files` (if needed)
   - Copy the generated URL
   - Paste it in your browser and authorize the bot

4. **Get the Channel ID**

   - Enable Developer Mode in Discord (User Settings → Advanced)
   - Right-click the channel where you want to receive messages
   - Select "Copy ID"

5. **Add Environment Variables**

   Create a `.env.local` file:

   ```bash
   DISCORD_BOT_TOKEN=your_bot_token_here
   DISCORD_CHANNEL_ID=your_channel_id_here
   ```

6. **Restart the Development Server**

   ```bash
   bun run dev
   ```

## Environment Variables Reference

| Variable | Required | Description |
|----------|-----------|-------------|
| `DISCORD_WEBHOOK_URL` | Yes (or use Bot API) | Your Discord webhook URL |
| `DISCORD_BOT_TOKEN` | Yes (if not using webhook) | Your Discord bot token |
| `DISCORD_CHANNEL_ID` | Yes (if not using webhook) | The channel ID to send messages to |

## Testing the Contact Form

1. Open your website at `http://localhost:3000`
2. Scroll to the "Contact" section
3. Fill out the form with test data
4. Click "Send Message"
5. Check your Discord channel for the message

## Message Format

When someone submits the form, you'll receive a beautiful embed in Discord:

```
📬 New Contact Form Message

👤 Name: John Doe
📧 Email: john@example.com
💬 Message: Your message here...

Sent from Skirk Bot Website • 2025-01-15
```

## Troubleshooting

### Webhook Not Working

- **Problem**: Messages not appearing in Discord
- **Solution**:
  - Verify the webhook URL is correct
  - Check the channel permissions
  - Ensure the webhook is not deleted
  - Try creating a new webhook

### Bot Not Working

- **Problem**: Bot not sending messages
- **Solution**:
  - Verify bot token is correct (no extra spaces)
  - Check bot has "Send Messages" permission in the channel
  - Ensure bot is invited to the correct server
  - Check Discord API status (https://discordstatus.com)

### Rate Limiting

- **Problem**: Messages not sending after several tries
- **Solution**:
  - Discord has rate limits. Wait a few seconds before retrying
  - Consider using webhooks instead (higher rate limits)

### Environment Variables Not Loading

- **Problem**: Server not reading `.env.local`
- **Solution**:
  - Ensure `.env.local` is in the project root
  - Restart the dev server after adding variables
  - Check there are no syntax errors in the file

## Security Best Practices

1. **Never Commit `.env` Files**
   ```bash
   # Add to .gitignore
   .env
   .env.local
   .env.production
   ```

2. **Use Environment-Specific Variables**
   - Development: `.env.local`
   - Production: Set via your hosting platform (Vercel, Netlify, etc.)

3. **Rotate Webhook URLs Regularly**
   - If a webhook is compromised, create a new one
   - Delete old webhooks from Discord

4. **Limit Bot Permissions**
   - Only grant minimum required permissions
   - Review permissions regularly

## Production Deployment

### Vercel

1. Go to your Vercel project settings
2. Add environment variables:
   - `DISCORD_WEBHOOK_URL` (or bot token/channel ID)
3. Redeploy the project

### Other Platforms

Follow your hosting platform's documentation for adding environment variables.

## Additional Resources

- [Discord Webhooks Documentation](https://discord.com/developers/docs/resources/webhook)
- [Discord Bot API Documentation](https://discord.com/developers/docs/intro)
- [Rate Limits](https://discord.com/developers/docs/topics/rate-limits)

## Support

If you encounter issues:
1. Check the browser console for errors
2. Check the server logs (`dev.log`)
3. Verify Discord API is working (test webhook URL in Postman)
4. Contact the development team
