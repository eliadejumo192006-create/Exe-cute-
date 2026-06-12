# WhatsApp Bot Deployment Guide

## Prerequisites
1. **OpenAI Account**: Sign up at https://openai.com and get your API key
2. **Twilio Account**: Create an account at https://www.twilio.com
3. **WhatsApp Business Account**: Set up through Twilio
4. **Hosting Service**: Choose one:
   - Heroku (free tier available)
   - AWS
   - DigitalOcean
   - Render
   - Railway

## Step 1: Get Credentials

### OpenAI API Key
1. Visit https://platform.openai.com/account/api-keys
2. Create a new API key
3. Copy and save it securely

### Twilio Credentials
1. Visit https://console.twilio.com
2. Note your Account SID and Auth Token
3. Go to WhatsApp section to get your WhatsApp number

## Step 2: Set Up Twilio WhatsApp

1. In Twilio Console, navigate to **Messaging > Try it out > Send a WhatsApp message**
2. Link your WhatsApp Business Account
3. Get your Twilio WhatsApp number
4. Note the webhook URL format: `https://your-domain.com/webhook`

## Step 3: Deploy on Heroku

### Option A: Using Git
```bash
# Install Heroku CLI
brew tap heroku/brew && brew install heroku  # macOS
# or visit https://devcenter.heroku.com/articles/heroku-cli

# Login to Heroku
heroku login

# Create app
heroku create your-whatsapp-bot-name

# Add buildpack for Python
heroku buildpacks:add heroku/python

# Set environment variables
heroku config:set OPENAI_API_KEY=your_openai_api_key
heroku config:set TWILIO_ACCOUNT_SID=your_account_sid
heroku config:set TWILIO_AUTH_TOKEN=your_auth_token
heroku config:set TWILIO_WHATSAPP_NUMBER=whatsapp:+1234567890
heroku config:set WEBHOOK_VERIFY_TOKEN=your_random_token
heroku config:set FLASK_ENV=production

# Push to Heroku
git push heroku main

# View logs
heroku logs --tail
```

### Option B: Using Docker
```bash
# Create a heroku.yml file
heroku create your-whatsapp-bot-name

# Set stack to container
heroku stack:set container

# Set environment variables (same as above)

# Deploy
git push heroku main
```

## Step 4: Configure Twilio Webhook

1. In Twilio Console, go to **Messaging > WhatsApp Senders**
2. Select your WhatsApp number
3. Under "When a message comes in", set:
   - Webhook URL: `https://your-app-name.herokuapp.com/webhook`
   - Method: POST
4. Save

## Step 5: Test Your Bot

1. Send a WhatsApp message to your Twilio number
2. Check if you receive a response
3. Monitor logs: `heroku logs --tail`

## Alternative Deployments

### Deploy on AWS Lambda + API Gateway
1. Package the app: `pip install -r requirements.txt -t package/`
2. Copy app files to `package/`
3. Zip: `zip -r lambda_function.zip package/`
4. Create Lambda function
5. Set environment variables in Lambda console
6. Create API Gateway endpoint
7. Set API Gateway URL as Twilio webhook

### Deploy on DigitalOcean App Platform
1. Push code to GitHub
2. Create new App in DigitalOcean
3. Connect GitHub repository
4. Set environment variables
5. Deploy
6. Use generated URL for Twilio webhook

### Deploy on Render
1. Push code to GitHub
2. Create new Web Service on Render
3. Connect GitHub repository
4. Set environment variables
5. Deploy
6. Use generated URL for Twilio webhook

## Monitoring and Maintenance

### Check Logs
```bash
heroku logs --tail  # Heroku
```

### Monitor Usage
- Check OpenAI API usage at https://platform.openai.com/account/usage/overview
- Check Twilio usage at https://console.twilio.com

### Update Bot Code
```bash
# Make changes to local files
git add .
git commit -m "Update bot functionality"
git push heroku main
```

## Troubleshooting

### Bot Not Responding
1. Check Heroku logs: `heroku logs --tail`
2. Verify Twilio webhook URL is correct
3. Test webhook manually: `curl -X POST https://your-app-name.herokuapp.com/webhook`

### OpenAI API Errors
1. Verify API key is correct
2. Check API usage limits
3. Monitor account balance

### Twilio Connection Issues
1. Verify Account SID and Auth Token
2. Check WhatsApp number format (should start with `whatsapp:+`)
3. Ensure webhook URL is publicly accessible

## Production Checklist
- [ ] API keys are set as environment variables (not in code)
- [ ] App is deployed to a production service
- [ ] Twilio webhook is configured correctly
- [ ] Error handling is implemented
- [ ] Logs are monitored
- [ ] Rate limiting is considered
- [ ] Bot personality/system prompt is customized
- [ ] Message timeout is appropriate

## Cost Considerations
- **OpenAI**: $0.0005 per 1K tokens (varies by model)
- **Twilio**: $0.0075 per message (WhatsApp)
- **Heroku**: Free tier (limited) or $7+/month
- **Other Services**: Check pricing on respective platforms

## Next Steps
1. Customize system prompt in `services/openai_service.py`
2. Add message persistence/logging
3. Implement rate limiting
4. Add more sophisticated error handling
5. Create user session management
6. Add admin commands
