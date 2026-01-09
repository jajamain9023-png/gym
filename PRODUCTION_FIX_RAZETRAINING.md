# PRODUCTION DEPLOYMENT FIX - razetraining.com

## Issues Fixed

### 1. ✅ Environment Variables Updated for Production

**Frontend .env:**
```bash
# BEFORE (Preview URL - WRONG for production):
REACT_APP_BACKEND_URL=https://gym-recovery-1.preview.emergentagent.com

# AFTER (Production URL - CORRECT):
REACT_APP_BACKEND_URL=https://razetraining.com
```

**Backend .env:**
```bash
# BEFORE (Preview URL - WRONG for production):
CORS_ORIGINS="https://gym-recovery-1.preview.emergentagent.com,https://raze11.app.n8n.cloud"

# AFTER (Production URLs - CORRECT):
CORS_ORIGINS="https://razetraining.com,https://www.razetraining.com,https://raze11.app.n8n.cloud"
```

### 2. ✅ Giveaway Webhook Verified Working

**Test Results:**
```
✅ API endpoint working: /api/emails/subscribe
✅ Webhook sent successfully: "n8n giveaway webhook sent successfully for giveawaytest@example.com"
✅ Status: HTTP/1.1 200 OK
```

The backend IS sending the webhook to n8n. If you're not receiving emails, the issue is in your n8n workflow.

---

## DEPLOYMENT REQUIRED

You **MUST** redeploy with the updated environment variables:

### Step 1: Save to GitHub
```bash
git add /app/frontend/.env /app/backend/.env
git commit -m "Update env vars for production domain razetraining.com"
git push origin main
```

### Step 2: Update Emergent Platform Environment Variables

In your Emergent deployment settings, set:

**Frontend Environment Variables:**
- `REACT_APP_BACKEND_URL` = `https://razetraining.com`

**Backend Environment Variables:**
- `CORS_ORIGINS` = `https://razetraining.com,https://www.razetraining.com,https://raze11.app.n8n.cloud`

### Step 3: Deploy

Click "Deploy" in Emergent platform and wait for completion.

---

## Giveaway Email Not Received - n8n Troubleshooting

The backend is successfully sending webhooks to:
```
https://raze11.app.n8n.cloud/webhook/raze-giveaway-entry
```

### Check Your n8n Workflow:

1. **Log into n8n:** https://raze11.app.n8n.cloud

2. **Find the giveaway workflow:**
   - Look for workflow with webhook: `/webhook/raze-giveaway-entry`

3. **Verify workflow is ACTIVE:**
   - Click on the workflow
   - Check the toggle in top right is ON (blue)
   - If it's OFF (gray), turn it ON

4. **Check webhook configuration:**
   - Webhook should accept: `email`, `event_type`, `timestamp`
   - Webhook method: POST
   - Content type: JSON

5. **Test the workflow:**
   - In n8n, click "Execute Workflow"
   - Or use test data in webhook node

6. **Check email node configuration:**
   - Verify "To" field uses `{{$json["email"]}}`
   - Verify email service is connected (Gmail, SendGrid, etc.)
   - Check if test mode is disabled

### Common n8n Issues:

**Issue 1: Workflow Not Active**
- **Solution:** Toggle workflow to ACTIVE in n8n dashboard

**Issue 2: Email Service Not Connected**
- **Solution:** Connect your email service in n8n credentials
- Options: Gmail, SendGrid, Mailgun, Resend, etc.

**Issue 3: Test Mode Enabled**
- **Solution:** Disable test mode in email node

**Issue 4: Wrong Email Template**
- **Solution:** Verify email body has the giveaway message

**Issue 5: Webhook URL Changed**
- **Solution:** Verify the webhook URL in n8n matches:
  ```
  https://raze11.app.n8n.cloud/webhook/raze-giveaway-entry
  ```

---

## Testing After Deployment

### Test 1: Signup Flow
```bash
# Visit your site
https://razetraining.com/register

# Open browser console (F12)
# Fill form with NEW email
# Click "Create Account"

# Should see in console:
🔍 API_URL loaded: https://razetraining.com
🔍 Register called with: {email: ..., name: ...}
🔍 Register response status: 200
```

### Test 2: Giveaway Entry
```bash
# Visit homepage
https://razetraining.com

# Find giveaway popup
# Enter email
# Submit

# Check n8n executions:
# - Go to n8n dashboard
# - Click "Executions" 
# - Look for recent execution with your email
```

### Test 3: CORS Verification
```javascript
// Open browser console on razetraining.com
fetch('https://razetraining.com/api/', {credentials: 'include'})
  .then(r => r.json())
  .then(data => console.log('✅ API working:', data))
  .catch(e => console.log('❌ CORS error:', e))
```

---

## Backend Logs Show Success

```
✅ Signup webhooks working:
   "n8n webhook sent successfully for newtest@example.com"

✅ Waitlist webhooks working:
   "n8n waitlist webhook sent successfully for waitlisttest@example.com"

✅ Giveaway webhooks working:
   "n8n giveaway webhook sent successfully for giveawaytest@example.com"
```

All webhooks are being sent with HTTP 200 OK responses. If emails aren't arriving, the issue is in n8n workflow configuration or email service connection.

---

## Quick Fix Checklist

- [x] Updated REACT_APP_BACKEND_URL to razetraining.com
- [x] Updated CORS_ORIGINS to include razetraining.com
- [x] Verified giveaway webhook is being sent
- [ ] **YOU NEED TO:** Redeploy to Emergent platform
- [ ] **YOU NEED TO:** Activate n8n giveaway workflow
- [ ] **YOU NEED TO:** Connect email service in n8n
- [ ] **YOU NEED TO:** Test signup with new email
- [ ] **YOU NEED TO:** Test giveaway entry

---

## If Still Having Issues

**Signup still failing?**
1. Clear browser cache (Ctrl+Shift+R)
2. Use NEW email (not previously registered)
3. Check console for exact error
4. Verify you deployed with updated .env files

**Giveaway emails not sending?**
1. Check n8n workflow is ACTIVE
2. Verify email service credentials in n8n
3. Check n8n execution logs for errors
4. Test the workflow manually in n8n

**Need the webhook payload format?**
```json
{
  "email": "user@example.com",
  "event_type": "giveaway_entry",
  "timestamp": "2026-01-09T20:49:02.993Z"
}
```

---

**Updated:** January 9, 2026  
**Production Domain:** razetraining.com  
**Status:** Environment variables updated, deployment required
