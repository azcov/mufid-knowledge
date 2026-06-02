---
tags: [requirements, specs]
---

# Functional Specs — DeenDaily

## Core Features

### Daily Check-In
- Main screen: list of today's habits to complete
- Tap each habit: done / skipped
- Target: < 30 seconds to complete full check-in

### Salah Tracking
- 5 prayers: Fajr, Dhuhr, Asr, Maghrib, Isya
- Per prayer: on-time / qadha / missed
- Core free feature

### Preset Sunnah Habits
- Quran recitation (pages or ayat)
- Morning adhkar
- Evening adhkar
- Sunnah fast (Mon/Thu)
- Pre-configured, no setup required — zero friction

### Custom Habits
- Free: up to 3 custom habits
- Paid: unlimited
- Daily yes/no (no complex tracking)

### Streak & History
- Consecutive-day streak counter (main gamification)
- Free: 7-day history view only
- Paid: unlimited history

### Analytics (Paid)
- Monthly heatmap (calendar view)
- Per-habit completion rate
- Best streaks
- Weekly plain-language insights ("You completed salah 5/5 on 18 of 28 days")

### Push Notifications
- Daily check-in reminder at user's chosen time
- Core retention driver

## User Flows

### First-Time User
1. Download app → sign up (email/Google/Apple)
2. See daily check-in with preset salah + sunnah habits
3. Tap each habit → done → streak starts (day 1)
4. Day 7: see 7-day streak → emotional hook → return daily

### Upgrade
1. Try to add 4th custom habit → hit free limit
2. "Upgrade to Premium" prompt
3. IAP via App Store / Play Store
4. Unlimited habits + full history unlocked

### Daily Habit Check-In
1. Open app → default screen is today's check-in
2. Tap salah status (on-time / qadha / missed) for each prayer
3. Tap done for each sunnah habit
4. Tap save → streak updates → motivational streak message shown

## Edge Cases
- Server-side IAP receipt validation (App Store / Play Store) — never trust client
- Freemium limits at usecase layer, not only frontend
- Streak: if user checks in at 11:59pm and again at 12:01am — count as two days (daily, not 24h)
- Apple Sign-In required for iOS App Store approval
