# Render-এ NOVAMART Deploy করার নির্দেশনা

## Render-এর তিনটি ঘর

যদি GitHub রিপোজিটরির মূল ফোল্ডারের ভিতরে `backend/manage.py` থাকে:

**Root Directory**
```text
backend
```

যদি `manage.py` সরাসরি GitHub রিপোজিটরির মূল ফোল্ডারেই থাকে, Root Directory খালি রাখুন।

**Build Command**
```bash
pip install -r requirements.txt
```

**Start Command**
```bash
python manage.py migrate --run-syncdb && python manage.py seed_payment_methods && gunicorn config.wsgi:application --bind 0.0.0.0:$PORT
```

## Environment Variables অবশ্যই সেট করুন
- `DJANGO_SECRET_KEY`: দীর্ঘ, র‍্যান্ডম secret key
- `DJANGO_DEBUG`: `False`
- `DJANGO_ALLOWED_HOSTS`: Render backend hostname (https:// ছাড়া)
- `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_HOST`, `POSTGRES_PORT`: Render PostgreSQL database-এর মান
- `CORS_ALLOWED_ORIGINS`: deployed frontend origin, যেমন `https://your-frontend.onrender.com`
- `CSRF_TRUSTED_ORIGINS`: deployed frontend origin-সহ `https://`
- `FRONTEND_BASE_URL`: deployed frontend origin

**খেয়াল রাখবেন:** ZIP-এর বাইরের ফোল্ডার `NOVAMART_Complete_MVP` GitHub-এ ঠিক একই নামে আছে কি না নিশ্চিত না হয়ে Root Directory-তে পুরো পথ দেবেন না।

## এই v4-এ সংশোধন
- অর্ডার API-তে undefined `method`, `transaction_id` এবং missing payment model imports ঠিক করা হয়েছে।
- নির্বাচিত payment method সক্রিয় কি না এবং manual payment reference দেওয়া হয়েছে কি না যাচাই করা হয়।
- Render-এর জন্য `gunicorn` dependency যোগ করা হয়েছে।

## এখনও production-এর আগে বাকি
বাস্তব পেমেন্ট গেটওয়ে/webhook, seller permissions/dashboard, guest chat authorization, email provider, shipping/refund/commission, version-controlled migrations, backup/logging এবং end-to-end/security tests এখনও দরকার। এই প্যাকেজকে production-ready বা ১০০% secure বলা যাবে না।
