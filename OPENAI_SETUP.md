# ✨ OPENAI API CONFIGURATION GUIDE

Complete guide to set up and configure the OpenAI API for your Psicología Bot.

---

## 📋 Prerequisites

- An OpenAI account (https://platform.openai.com)
- A valid payment method (even for free trial)
- Basic understanding of API keys

---

## 🚀 Step-by-Step Setup

### Step 1: Create OpenAI Account

1. Visit: https://platform.openai.com/signup
2. Sign up with your email or Google/Microsoft account
3. Verify your email address
4. Add your phone number
5. Set up billing (required even for free tier)

### Step 2: Generate API Key

1. Go to: https://platform.openai.com/api-keys
2. Click **"Create new secret key"**
3. Choose a name (e.g., "Psychology Bot")
4. Copy your key immediately (you won't see it again!)
5. Store it safely (don't share with anyone)

### Step 3: Set Up Billing

1. Go to: https://platform.openai.com/account/billing/overview
2. Add a payment method
3. Set up usage limits to control costs:
   - Go to: https://platform.openai.com/account/billing/limits
   - Set a **Hard limit** (e.g., $50/month)
   - This prevents unexpected charges

### Step 4: Check API Usage

1. Visit: https://platform.openai.com/account/usage/overview
2. Monitor your API calls and costs
3. Adjust usage limits if needed

---

## 💰 Pricing Overview

### Current Pricing (as of 2024)

| Model | Input | Output |
|-------|-------|--------|
| GPT-3.5 Turbo | $0.0005/1K tokens | $0.0015/1K tokens |
| GPT-4 | $0.03/1K tokens | $0.06/1K tokens |
| GPT-4 Turbo | $0.01/1K tokens | $0.03/1K tokens |

### Estimate Costs

**Example: 1000 conversations/month**
- Avg 100 tokens per response
- Using GPT-3.5 Turbo: ~$0.15/conversation
- **Total: ~$150/month**

### Cost Optimization Tips

1. **Use GPT-3.5 Turbo** instead of GPT-4 (much cheaper)
2. **Reduce token count:**
   - Shorter system prompts
   - Limit max_tokens in API call
   - Use fewer conversation messages
3. **Cache responses** for common questions
4. **Use knowledge base first** before calling OpenAI
5. **Monitor usage** regularly

---

## 🔧 Configure Your App

### Local Development

1. **Create `.env` file:**
   ```bash
   cp .env.example .env
   ```

2. **Add your API key:**
   ```env
   OPENAI_API_KEY=sk-your-key-here
   ```

3. **Verify it works:**
   ```bash
   python -c "import openai; openai.api_key = 'sk-your-key'; print('✅ API Key configured!')"
   ```

### Production Deployment

**Railway:**
```bash
# In Railway Dashboard → Variables
OPENAI_API_KEY=sk-your-key-here
```

**Heroku:**
```bash
heroku config:set OPENAI_API_KEY=sk-your-key-here
```

**Docker/Cloud Run:**
```bash
docker run -e OPENAI_API_KEY=sk-your-key-here psicologia-bot:latest
```

---

## 🧪 Test Your Configuration

### Quick Test

```python
import openai

openai.api_key = "sk-your-key-here"

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
        {
            "role": "system",
            "content": "Eres un asistente de psicología."
        },
        {
            "role": "user",
            "content": "Hola, tengo ansiedad"
        }
    ],
    max_tokens=100
)

print(response.choices[0].message['content'])
```

### Test with curl

```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-your-key-here" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [
      {
        "role": "user",
        "content": "Hola"
      }
    ],
    "max_tokens": 50
  }'
```

---

## 🔒 Security Best Practices

### ✅ DO's

- ✅ Store API key in `.env` file
- ✅ Never commit `.env` to Git
- ✅ Use different keys for dev/prod
- ✅ Rotate keys periodically
- ✅ Monitor usage regularly
- ✅ Set hard spending limits

### ❌ DON'Ts

- ❌ Hardcode API key in source code
- ❌ Share key via email or chat
- ❌ Commit `.env` file to Git
- ❌ Expose key in frontend code
- ❌ Use same key across projects

---

## 🛡️ API Key Management

### Rotate Your Key

1. Go to: https://platform.openai.com/api-keys
2. Click the **trash icon** to delete old key
3. Create a new key
4. Update your `.env` file
5. Redeploy your app

### Multiple Keys

```bash
# Development
OPENAI_API_KEY_DEV=sk-dev-key...

# Production
OPENAI_API_KEY_PROD=sk-prod-key...

# In your app
API_KEY = os.getenv('OPENAI_API_KEY_PROD' if is_production else 'OPENAI_API_KEY_DEV')
```

---

## 🚨 Troubleshooting

### Error: "Invalid API Key"

**Solution:**
1. Check your key is correct
2. Verify it's not expired
3. Try regenerating a new key
4. Ensure no extra spaces in the key

### Error: "Rate limit exceeded"

**Solution:**
1. Implement exponential backoff
2. Cache responses
3. Use knowledge base first
4. Upgrade your OpenAI plan

### Error: "Insufficient quota"

**Solution:**
1. Check billing settings
2. Add payment method if needed
3. Check usage limits
4. Request quota increase

### Error: "Model not found"

**Solution:**
1. Use correct model name: `gpt-3.5-turbo`
2. Check model availability in your region
3. Verify API key has access to model

---

## 📊 Monitor Your Usage

### Using OpenAI Dashboard

1. Go to: https://platform.openai.com/account/usage/overview
2. Check API costs
3. View requests by day/month
4. Export usage data

### Using Python

```python
import openai

# Get remaining credits (if available)
# Note: This endpoint may not be available
try:
    usage = openai.Usage.retrieve()
    print(f"Total used: ${usage.total_usage / 100}")
except Exception as e:
    print(f"Error: {e}")
```

---

## ⚙️ Advanced Configuration

### Custom System Prompt

```python
SYSTEM_PROMPT = """Eres un asistente especializado en psicología.
- Responde en español
- Sé empático y comprensivo
- Siempre recomienda consultar con profesionales para casos graves
- Limita tus respuestas a 300 palabras
"""
```

### Token Optimization

```python
def count_tokens(text):
    """Estimate tokens in text"""
    return len(text) // 4  # Rough estimate

# Limit max tokens based on usage
if count_tokens(user_message) > 100:
    max_tokens = 200
else:
    max_tokens = 500
```

### Caching Responses

```python
from functools import lru_cache
import json

@lru_cache(maxsize=1000)
def get_cached_response(message):
    """Cache API responses to save costs"""
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": message}],
        max_tokens=300
    )
    return response.choices[0].message['content']
```

---

## 📚 Useful Resources

- [OpenAI API Docs](https://platform.openai.com/docs)
- [API Reference](https://platform.openai.com/docs/api-reference)
- [Pricing](https://openai.com/pricing)
- [Model Comparison](https://platform.openai.com/docs/models)
- [Safety Best Practices](https://platform.openai.com/docs/guides/safety-best-practices)

---

## ✅ Checklist

- [ ] OpenAI account created
- [ ] API key generated
- [ ] Billing method added
- [ ] Spending limits configured
- [ ] `.env` file created with API key
- [ ] App tested with OpenAI
- [ ] Key secured and not committed to Git
- [ ] Monitoring set up
- [ ] Cost optimization implemented

---

**Your Psicología Bot is now ready to use OpenAI! 🤖**