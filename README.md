# 🏛️ PoolHub — কেন্দ্রীয়-প্রক্সি-ব্যবস্থাপনা (আলাদা-অ্যাডন)

ধ্রুব-প্রক্সি-স্ক্যান → alive-পুল (৩০০) → **৫-সাব-স্পেস** (condition-pass=bench) → **প্রতি-স্পেস-API-কী** → গ্রাহক-অ্যাপ-নিজ-কী-দিয়ে-সংগ্রহ+ব্যবহার।

## কাঠামো

```
স্ক্যান (TheSpeedX/monosans/proxyscrape/proxifly) → পুল (alive, ৩০০-ক্যাপ)
   → ৫-সাব-স্পেস: toonworld4all · raretoons · vidy · moviebox · multimovies
      শর্ত: robots+wp-admin / robots+wp-admin / seed-API / টোকেন-সংগ্রহ (x-user-JWT) / resolve-json
   → প্রতি-স্পেসে-ইউনিক-কী (poolhub_keys.json, psk-…)
   → গ্রাহক: vidy [vidy·moviebox·multimovies] · রেজলভার [toonworld4all·raretoons]
```

**প্রক্সি-নীতি**: প্রক্সি=শুধু-IP/জিও-ফ্ল্যাগ-বাইপাস (টোকেন/API/ম্যানিফেস্ট, বডি-ক্যাপ-২৫৬KB) — **মিডিয়া-প্লেব্যাক-কখনো-প্রক্সি-দিয়ে-নয়**; প্লেয়ার-সরাসরি-CDN।

## API

| রুট | বিবরণ |
|---|---|
| `GET /health` | পুল+স্পেস+খরচ-গণক |
| `GET /spaces?key=` | কী=স্পেস-নির্ধারক (শুধু-নিজ-স্পেস); অ্যাডমিন-টোকেনে-সব |
| `POST /register` | অ্যাপ-নিজ-শর্ত-নিবন্ধন (কী বা src) |
| `GET /fetch?src=&url=&key=` | নিজ-স্পেস-px-দিয়ে-ফেচ |
| `GET /pick?src=&n=&key=` | নিজ-স্পেসের-সেরা-px |
| `POST /report` | গ্রাহক-প্রমাণ-ফিড (px, ok, ms, src) |
| `POST /token` (অ্যাডমিন) | টোকেন-ইউজার → `u:<name>`-স্পেস + `/my/*` |

অ্যাডমিন-টোকেন: `POOLHUB_ADMIN_TOKEN` env বা `poolhub_admin.txt` (অটো-জেন)।

## চালানো

```bash
PORT=8399 node server.js          # লোকাল
git push dokku HEAD:master        # beamup: 3404d3c5dc63-poolhub
```
