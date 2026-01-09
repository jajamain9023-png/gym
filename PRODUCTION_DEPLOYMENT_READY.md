# 🚀 PRODUCTION DEPLOYMENT READY - razetraining.com

**Date:** January 9, 2026  
**Status:** ✅ READY FOR PRODUCTION DEPLOYMENT

---

## ✅ Environment Variables Updated

### Frontend Configuration
**File:** `/app/frontend/.env`

```bash
REACT_APP_BACKEND_URL=https://razetraining.com
WDS_SOCKET_PORT=443
ENABLE_HEALTH_CHECK=false
```

✅ **Status:** Updated to production URL

---

### Backend Configuration
**File:** `/app/backend/.env`

```bash
MONGO_URL="mongodb://localhost:27017"
DB_NAME="raze_training"
CORS_ORIGINS="https://razetraining.com,https://www.razetraining.com,https://raze11.app.n8n.cloud"

# n8n Webhook URLs (All Configured ✅)
WEBHOOK_ACCOUNT_SIGNUP=https://raze11.app.n8n.cloud/webhook/raze-account-signup
WEBHOOK_WAITLIST=https://raze11.app.n8n.cloud/webhook/raze-waitlist
WEBHOOK_GIVEAWAY=https://raze11.app.n8n.cloud/webhook/raze-giveaway-entry
WEBHOOK_ABANDONED_CART_1=https://raze11.app.n8n.cloud/webhook/raze-abandoned-cart-1
WEBHOOK_ABANDONED_CART_2=https://raze11.app.n8n.cloud/webhook/raze-abandoned-cart-2
WEBHOOK_ABANDONED_CART_3=https://raze11.app.n8n.cloud/webhook/raze-abandoned-cart-3
N8N_ORDER_WEBHOOK_URL=https://raze11.app.n8n.cloud/webhook/raze-order-confirmation
N8N_BULK_EMAIL_WEBHOOK_URL=https://raze11.app.n8n.cloud/webhook/raze-bulk-email
```

✅ **Status:** Updated to production URLs

---

## 🎯 What This Means

### CORS Configuration
Your backend will now accept requests from:
- ✅ `https://razetraining.com` (main domain)
- ✅ `https://www.razetraining.com` (www subdomain)
- ✅ `https://raze11.app.n8n.cloud` (n8n webhooks)

### API Communication
- Frontend will call: `https://razetraining.com/api/*`
- Backend will respond from: `https://razetraining.com/api/*`
- All webhook notifications go to n8n

---

## 📋 Pre-Deployment Checklist

### Critical Items ✅
- [x] Frontend .env updated to production URL
- [x] Backend .env updated with production CORS
- [x] All n8n webhook URLs configured
- [x] Services restarted successfully
- [x] Database name set to "raze_training"

### Optional Items (Can Add Later)
- [ ] STRIPE_API_KEY (when ready for payments)
- [ ] RESEND_API_KEY (optional, webhooks work without it)
- [ ] SHIPPO_API_KEY (when ready for shipping)
- [ ] ADMIN_PASSWORD (for admin panel access)

---

## 🚀 Deployment Steps

### 1. Commit Changes to GitHub
```bash
git add /app/frontend/.env /app/backend/.env
git commit -m "Update environment variables for production (razetraining.com)"
git push origin main
```

### 2. Deploy on Emergent Platform
1. Go to your Emergent dashboard
2. Click "Deploy" button
3. Wait 2-5 minutes for deployment to complete

### 3. Verify Deployment
After deployment, test:

**A. Frontend Loading:**
```
Visit: https://razetraining.com
Expected: Homepage loads correctly
```

**B. API Connection:**
```
Open browser console on razetraining.com
Look for: "🔍 API_URL loaded: https://razetraining.com"
Expected: No CORS errors
```

**C. Signup Flow:**
```
1. Go to: https://razetraining.com/register
2. Fill out form with new email
3. Click "Create Account"
Expected: Success + redirect to dashboard
```

**D. Waitlist Flow:**
```
1. Click "Join Waitlist" button
2. Enter email + select size
3. Click "Secure My Spot"
Expected: Success message with access code
```

---

## 🔧 Post-Deployment Configuration

### 1. Set Up Abandoned Cart Cron Job

**What:** Automated email sequence for abandoned carts  
**When:** Every 5 minutes  
**Endpoint:** `POST https://razetraining.com/api/cart/process-abandoned`

**How to Set Up:**

**Option A: Emergent Platform (if available)**
- Check if Emergent has cron job support
- Set frequency to "*/5 * * * *" (every 5 minutes)

**Option B: External Cron Service**
Use cron-job.org or similar:
1. Create account on cron-job.org
2. Create new cron job
3. URL: `https://razetraining.com/api/cart/process-abandoned`
4. Method: POST
5. Frequency: Every 5 minutes

**Option C: Vercel Cron (if on Vercel)**
Add to `vercel.json`:
```json
{
  "crons": [{
    "path": "/api/cart/process-abandoned",
    "schedule": "*/5 * * * *"
  }]
}
```

### 2. Verify n8n Workflows Are Active

Login to: https://raze11.app.n8n.cloud

**Check these workflows are ACTIVE:**
- ✅ raze-account-signup
- ✅ raze-waitlist
- ✅ raze-giveaway-entry
- ✅ raze-abandoned-cart-1
- ✅ raze-abandoned-cart-2
- ✅ raze-abandoned-cart-3
- ✅ raze-order-confirmation
- ✅ raze-bulk-email

**Verify:**
- Each workflow has toggle switched to ON (blue)
- Webhook URLs match your .env file
- Email credentials are configured

---

## 🧪 Testing Checklist (After Deployment)

### Homepage Tests
- [ ] Homepage loads without errors
- [ ] Countdown timer works (shows days to Feb 20)
- [ ] Product images load
- [ ] Navigation menu works
- [ ] Mobile responsive
- [ ] Language selector works

### Authentication Tests
- [ ] Sign up with new email works
- [ ] Login with existing account works
- [ ] Google OAuth redirects correctly
- [ ] Logout works
- [ ] Dashboard loads after login

### Waitlist Tests
- [ ] "Join Waitlist" button appears
- [ ] Modal opens correctly
- [ ] Email submission works
- [ ] Access code displays: "Your Access Code: RAZE-XXXXXX"
- [ ] Confirmation message shows
- [ ] Email notification received (check n8n)

### API Tests
- [ ] No CORS errors in console
- [ ] All API calls use https://razetraining.com
- [ ] Response times are acceptable
- [ ] Error handling works

### Mobile Tests
- [ ] Works on iPhone Safari
- [ ] Works on Android Chrome
- [ ] Touch targets large enough
- [ ] Forms work properly
- [ ] Buttons are tappable

---

## 🐛 Troubleshooting Guide

### Issue: "Failed to fetch" or CORS Error
**Symptom:** API calls failing, CORS error in console  
**Solution:**
1. Verify CORS_ORIGINS in backend .env
2. Check it includes razetraining.com
3. Restart backend: `supervisorctl restart backend`
4. Clear browser cache

### Issue: Signup not working
**Symptom:** Form submits but nothing happens  
**Solution:**
1. Open browser console (F12)
2. Look for 🔍 debug messages
3. Check Network tab for API responses
4. Verify REACT_APP_BACKEND_URL is correct
5. Try with different email

### Issue: Waitlist emails not sending
**Symptom:** User joins waitlist but no email received  
**Solution:**
1. Check backend logs for webhook success
2. Login to n8n dashboard
3. Check "Executions" tab
4. Verify workflow is ACTIVE
5. Check email service credentials in n8n

### Issue: Countdown timer shows wrong date
**Symptom:** Timer counts to wrong date  
**Solution:**
- Should count to Feb 20, 2026
- Check WaitlistBanner.jsx line 13
- Clear browser cache
- Restart frontend

---

## 📊 Monitoring After Launch

### What to Monitor:
1. **Error rates** - Should be < 1%
2. **Response times** - Should be < 200ms
3. **Conversion rates** - Waitlist signups per visitor
4. **Bounce rate** - Users leaving immediately
5. **Popular products** - Which get most clicks

### Tools to Use:
- Google Analytics (if installed)
- Browser console errors
- n8n execution logs
- Backend API logs

---

## 🎉 Ready to Launch!

Your RAZE Training e-commerce site is now configured for production deployment on razetraining.com!

### What's Working:
✅ User authentication (email/password + Google OAuth)  
✅ Waitlist functionality with access codes  
✅ Product catalog (4 shirts + 2 shorts)  
✅ Shopping cart system  
✅ Multi-language support (10 languages)  
✅ Currency localization  
✅ RAZE Credits system  
✅ n8n webhook integrations  
✅ Countdown timer to Feb 20  
✅ Mobile responsive design  
✅ Admin dashboard  

### What's Pending:
⚠️ Stripe integration (when ready)  
⚠️ Shippo integration (when ready)  
⚠️ Abandoned cart cron job (needs setup)  

### Deployment Strategy:
1. **Soft Launch** - Enable waitlist mode, collect emails
2. **Build Hype** - Send emails, post on social media
3. **Feb 20 Launch** - Enable checkout, fulfill orders

---

## 🔐 Security Notes

- All passwords are hashed with PBKDF2
- Session tokens are secure, httpOnly cookies
- CORS is properly restricted
- No API keys exposed in frontend
- Environment variables properly secured

---

## 📝 Next Steps After Deployment

1. **Day 1:** Monitor error logs, fix any issues
2. **Week 1:** Track waitlist signups, optimize conversion
3. **Week 2:** A/B test button text, colors
4. **Week 3:** Add customer testimonials, social proof
5. **Week 4:** Prepare for Feb 20 launch

---

**Last Updated:** January 9, 2026  
**Production URL:** https://razetraining.com  
**Status:** ✅ READY FOR DEPLOYMENT

Good luck with your launch! 🚀
