# 📱 Namaz & Roza Tracker - Complete Project Plan
Version: 1.0
Created: 2026-03-02
Status: Planning Complete - Ready for Phase 1

═══════════════════════════════════════════════

## 🎯 APP KA MAQSAD (Purpose)

Yeh app Muslim users ko help karegi ke:
- Baligh hone se aaj tak kitni namaz farz hui
- Kitni ada ki, kitni qaza bachi
- Baligh hone se aaj tak kitne Ramadan rozay farz hue
- Kitne rakhe, kitne qaza bache
- Roz ki namaz aur roza track ho
- Google Drive pe backup rahe

═══════════════════════════════════════════════

## 📋 APP DETAILS

- Naam: Namaz & Roza Tracker
- Package: com.islamic.namazrozatracker
- Minimum Android: 8.0 (API 26)
- Target Android: 14 (API 34)
- Language: Kotlin
- UI: Jetpack Compose
- Architecture: MVVM

═══════════════════════════════════════════════

## 🛠️ TECHNOLOGY LIST

| Cheez           | Technology          | Kyun?                        |
|-----------------|---------------------|------------------------------|
| Language        | Kotlin              | Android ki best language     |
| UI              | Jetpack Compose     | Modern aur aasan             |
| Database        | Room (SQLite)       | Offline data save            |
| Prayer Times    | Adhan-java          | Offline calculate hoti hain  |
| Notifications   | WorkManager         | Background reminders         |
| Navigation      | Navigation Compose  | Screen se screen jana        |
| Google Backup   | Google Drive API    | WhatsApp jaisa backup        |
| Google Login    | Google Sign-In SDK  | Drive ke liye login          |
| Charts          | MPAndroidChart      | Progress graphs              |
| Architecture    | MVVM + Repository   | Clean code pattern           |

═══════════════════════════════════════════════

## 📱 SCREENS (App ke Pages)

### Screen 1: Splash Screen
- App ka naam aur logo
- Islamic design (green/gold)
- 2 second baad auto-next

### Screen 2: Setup Screen (Sirf pehli baar)
- User ka naam
- Baligh hone ki date (date picker)
- Gender (Male/Female)
- City select (prayer times ke liye)
- "Shuru Karo" button

### Screen 3: Home Dashboard
- Assalamualaikum [Naam]!
- Aaj ki tarikh (English + Islamic)
- Aaj ki namaz: [2/5 ✅]
- Aaj ka roza: [Rakha ✅ / Nahi ❌]
- Qaza namaz remaining: [1,245]
- Qaza roza remaining: [12]
- Quick buttons

### Screen 4: Namaz Screen
- Aaj ki 5 waqt namaz (Ada/Qaza buttons)
- Fajr | Zuhr | Asr | Maghrib | Isha
- ── Stats Section ──
- Baligh hone se aaj tak total farz: [X]
- Total ada ki: [X]
- Total QAZA baqi: [X]
- Manual adjust button (+/-)

### Screen 5: Roza Screen
- Aaj ka roza (Rakha/Nahi/Safar/Bimari)
- Ramadan calendar (current year)
- ── Stats Section ──
- Baligh hone se kitne Ramadan: [X]
- Total farz rozay: [X]
- Total rakhe: [X]
- Total QAZA baqi: [X]
- Manual adjust button (+/-)

### Screen 6: Progress Screen
- Namaz progress bar (X% complete)
- Roza progress bar (X% complete)
- Monthly calendar view
- Streak counter (lagatar kitne din)
- Weekly chart
- Monthly chart

### Screen 7: Settings Screen
- 👤 Profile (naam, baligh date change)
- 🔔 Notifications
-     └── Namaz reminders on/off
-     └── Sehri/Iftar reminder on/off
-     └── Daily qaza reminder on/off
- 💾 Backup & Restore
-     └── Google Drive on/off
-     └── Auto backup: Daily/Weekly/Monthly
-     └── "Abhi Backup Karo" button
-     └── "Restore Karo" button
-     └── Last backup time
- 🎨 Theme
-     └── Dark/Light mode
-     └── Language (Urdu/English)
- ℹ️ About App

═══════════════════════════════════════════════

## 🗂️ DATABASE DESIGN

### Table 1: user_profile
| Column         | Type    | Description              |
|----------------|---------|--------------------------|
| id             | Int     | Auto primary key         |
| name           | String  | User ka naam             |
| DOB            | String  | DOB DD-MM-YYYY format    |
| baligh_date    | String  | DD-MM-YYYY format        |
| gender         | String  | male / female            |
| mobile_num     | String  | mobile number            |
| city           | String  | Prayer times ke liye     |
| setup_complete | Boolean | Setup hua ya nahi        |
| created_at     | String  | Account banane ki date   |

### Table 2: namaz_log
| Column     | Type   | Description                    |
|------------|--------|--------------------------------|
| id         | Int    | Auto primary key               |
| date       | String | DD-MM-YYYY                     |
| fajr       | String | ada / qaza / not_marked        |
| zuhr       | String | ada / qaza / not_marked        |
| asr        | String | ada / qaza / not_marked        |
| maghrib    | String | ada / qaza / not_marked        |
| isha       | String | ada / qaza / not_marked        |

### Table 3: roza_log
| Column      | Type    | Description                        |
|-------------|---------|------------------------------------|
| id          | Int     | Auto primary key                   |
| date        | String  | DD-MM-YYYY                         |
| status      | String  | rakha/choot_gaya/safar/bimari      |
| is_ramadan  | Boolean | Ramadan ka din hai ya nahi         |
| notes       | String  | Optional notes                     |

### Table 4: qaza_summary
| Column            | Type   | Description              |
|-------------------|--------|--------------------------|
| id                | Int    | Auto primary key         |
| namaz_qaza_manual | Int    | User ne khud adjust kiya |
| roza_qaza_manual  | Int    | User ne khud adjust kiya |
| last_updated      | String | Last calculation time    |

═══════════════════════════════════════════════

## 🔢 QAZA CALCULATION LOGIC

### Namaz Qaza Formula:
```
Step 1: baligh_date se aaj tak total din count karo
Step 2: total_din × 5 = total_farz_namaz
Step 3: namaz_log se count karo kitni "ada" hain
Step 4: total_farz_namaz - ada_count = QAZA_REMAINING
Step 5: qaza_summary.namaz_qaza_manual bhi add karo
        (agar user ne manually adjust kiya ho)
```

### Roza Qaza Formula:
```
Step 1: baligh_date se aaj tak har saal ka 
        Ramadan check karo
Step 2: Har Ramadan 29 ya 30 din ka hota hai
        (Islamic calendar se)
Step 3: roza_log se count karo kitne "rakha" hain
        jo is_ramadan = true hain
Step 4: total_ramadan_days - rakhe_count = QAZA_REMAINING
Step 5: qaza_summary.roza_qaza_manual bhi add karo
```

═══════════════════════════════════════════════

## ⚠️ IMPORTANT DECISIONS

| Decision              | Choice                  |
|-----------------------|-------------------------|
| Baligh age - Boys     | 15 saal                 |
| Baligh age - Girls    | 12 saal                 |
| Prayer method         | Karachi / HEC (Pakistan)|
| Offline first         | Haan - 100% offline     |
| Internet needed       | Sirf backup ke liye     |
| Backup location       | Google Drive            |
| Min Android           | 8.0 (API 26)            |
| App languages         | Urdu + English          |
| Theme colors          | Green + Gold (Islamic)  |

═══════════════════════════════════════════════

## 📅 DEVELOPMENT PHASES

─────────────────────────────────────────
### 🚀 PHASE 1 - Foundation (Bunyaad)
─────────────────────────────────────────
Goal: App chalti ho, data save ho, basic UI ready ho

Kya banega:
- [ ] Android Studio project setup
- [ ] build.gradle dependencies add
- [ ] Room database (4 tables)
- [ ] MVVM architecture setup
- [ ] Splash Screen
- [ ] Setup Screen (naam, date, gender, city)
- [ ] Home Dashboard (basic)
- [ ] Namaz Screen (5 waqt mark karna)
- [ ] Roza Screen (daily mark karna)
- [ ] Bottom Navigation (4 tabs)

Files jo banegi:
```
app/src/main/
├── AndroidManifest.xml
├── java/com/islamic/namazrozatracker/
│   ├── MainActivity.kt
│   ├── data/
│   │   ├── database/AppDatabase.kt
│   │   ├── entities/UserProfile.kt
│   │   ├── entities/NamazLog.kt
│   │   ├── entities/RozaLog.kt
│   │   ├── entities/QazaSummary.kt
│   │   ├── dao/UserProfileDao.kt
│   │   ├── dao/NamazLogDao.kt
│   │   ├── dao/RozaLogDao.kt
│   │   └── repository/AppRepository.kt
│   ├── ui/
│   │   ├── screens/SplashScreen.kt
│   │   ├── screens/SetupScreen.kt
│   │   ├── screens/HomeScreen.kt
│   │   ├── screens/NamazScreen.kt
│   │   ├── screens/RozaScreen.kt
│   │   └── theme/Theme.kt
│   ├── viewmodel/
│   │   ├── SetupViewModel.kt
│   │   ├── HomeViewModel.kt
│   │   ├── NamazViewModel.kt
│   │   └── RozaViewModel.kt
│   └── navigation/
│       └── AppNavigation.kt
└── res/
    ├── values/colors.xml
    ├── values/strings.xml
    └── drawable/ic_launcher.xml
```

─────────────────────────────────────────
### 🧮 PHASE 2 - Qaza Calculator
─────────────────────────────────────────
Goal: Automatic qaza calculate ho baligh date se

Kya banega:
- [ ] QazaCalculator.kt (main logic file)
- [ ] Namaz qaza auto-calculate
- [ ] Roza qaza auto-calculate (Islamic calendar)
- [ ] Home screen pe live stats
- [ ] Manual adjust feature (+/- buttons)
- [ ] Stats update ho jab bhi namaz/roza mark ho

─────────────────────────────────────────
### 🔔 PHASE 3 - Notifications & Prayer Times
─────────────────────────────────────────
Goal: App khud yaad dilaye

Kya banega:
- [ ] Adhan-java library setup
- [ ] City se prayer times calculate (offline)
- [ ] 5 waqt namaz notifications
- [ ] Sehri time notification (Ramadan)
- [ ] Iftar time notification (Ramadan)
- [ ] Daily qaza reminder (raat ko)
- [ ] Notification permission handling

─────────────────────────────────────────
### 📊 PHASE 4 - Progress & Statistics
─────────────────────────────────────────
Goal: Visually dekhein kitna progress hua

Kya banega:
- [ ] Progress Screen complete
- [ ] Namaz progress bar + percentage
- [ ] Roza progress bar + percentage
- [ ] Monthly calendar (color coded)
- [ ] Streak counter (lagatar din)
- [ ] Weekly bar chart
- [ ] Monthly line chart
- [ ] MPAndroidChart library integrate

─────────────────────────────────────────
### 🎨 PHASE 5 - UI Polish + Backup
─────────────────────────────────────────
Goal: App sundar ho + Google Drive backup

Kya banega:
- [ ] Islamic theme finalize (Green + Gold)
- [ ] Dark mode
- [ ] App icon design
- [ ] Animations (smooth transitions)
- [ ] Settings screen complete
- [ ] Google Sign-In setup
- [ ] Google Drive API integrate
- [ ] Auto backup (WorkManager)
- [ ] Manual backup button
- [ ] Restore from Drive
- [ ] Language toggle (Urdu/English)

─────────────────────────────────────────
### 🏪 PHASE 6 - Play Store
─────────────────────────────────────────
Goal: App publish karna

Kya karna hoga:
- [ ] App test karna (bugs fix)
- [ ] Keystore banana (app sign karna)
- [ ] Release APK/AAB generate
- [ ] Play Store account ($25 one time)
- [ ] Screenshots banana (5-8)
- [ ] Feature graphic banana
- [ ] App description likhna (Urdu+English)
- [ ] Privacy policy banana
- [ ] Upload aur submit
- [ ] Review ka intezaar (2-3 din)

═══════════════════════════════════════════════

## 🤲 Dua

May Allah make this app a source of sadqa-e-jariya.
May it help Muslims fulfill their obligations.
Ameen.
