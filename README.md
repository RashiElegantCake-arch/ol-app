# 📚 O/L Champion - Sri Lanka G.C.E. O/L Revision App

A clean, responsive, and student-focused mobile application built with Flutter & Firebase, specifically tailored for Sri Lankan students preparing for their December G.C.E. Ordinary Level (O/L) examinations.

---

## 🌟 Key Features

1. **Prominent Live December O/L Countdown Timer**:
   - Calculates the exact remaining Days, Hours, Minutes, and Seconds until the December O/L exam (defaults to 1st Monday of December at 8:30 AM).
   - Academic year progress bar showing study trajectory.
   - Customizable exam date selector to accommodate Ministry schedule updates.

2. **Core Subject Hub**:
   - Covers core subjects and basket choices:
     - **Mathematics (ගණිතය / கணிதம்)**
     - **Science (විද්‍යාව / அறிவியல்)**
     - **History (ඉතිහාසය / வரலாறு)**
     - **Sinhala Language & Literature (සිංහල භාෂාව හා සාහිත්‍යය)**
     - **English Language**
     - **Business & Accounting Studies (ව්‍යාපාර හා ගිණුම්කරණය)**
     - **Information & Communication Technology (ICT)**
     - **Buddhism (බුද්ධ ධර්මය)**

3. **Short Notes Section**:
   - Unit-by-unit short notes viewer for Grade 10 & 11 syllabus.
   - Key formulas, bulleted exam takeaways, and high-yield summaries.
   - In-app PDF viewer integration with local document caching for offline revision.
   - Reader font size adjustments (A- / A+).

4. **Model Papers & Past Papers Hub**:
   - Subject-wise and year-wise past papers (2018–2025).
   - Official Marking Schemes and answer keys.
   - Full-featured PDF viewer with page indicators and zoom.

5. **Quick Search & Bookmark Vault**:
   - Global keyword search across notes, unit titles, and past papers.
   - Persistent offline bookmarking using `SharedPreferences`.

6. **Clean & Motivational UI/UX**:
   - Daily inspirational study quotes with Sinhala translations.
   - Dark Mode / Light Mode with automatic persistence.
   - Material 3 design and Google Fonts (Poppins).

---

## 🏗️ Architecture & File Structure

```
c:\Users\MCM\Desktop\OL Lesson\
├── android/
│   └── app/src/main/AndroidManifest.xml   # Internet & storage permissions
├── lib/
│   ├── main.dart                          # App entry point, MultiProvider & Firebase init
│   ├── constants/
│   │   ├── app_colors.dart                # Subject color tags, gradients & palettes
│   │   ├── app_strings.dart               # Student-friendly headers and strings
│   │   └── app_themes.dart                # Light & Dark Material 3 theme data
│   ├── models/
│   │   ├── subject_model.dart             # Subject entity (Sinhala/Tamil/English)
│   │   ├── note_model.dart                # Short notes with rich text & PDF references
│   │   ├── paper_model.dart               # Past/Model paper with marking scheme URLs
│   │   └── quote_model.dart               # Motivational quote entity
│   ├── services/
│   │   ├── firestore_service.dart         # Firestore querying + graceful offline fallback
│   │   ├── mock_data_service.dart         # Rich initial Sri Lankan syllabus data
│   │   └── storage_service.dart           # PDF downloader and offline caching manager
│   ├── providers/
│   │   ├── theme_provider.dart            # Theme switcher (Light/Dark mode)
│   │   ├── exam_timer_provider.dart       # Live December O/L countdown ticker
│   │   ├── bookmark_provider.dart         # Saved notes/papers manager
│   │   └── content_provider.dart          # Subjects, notes, papers & search state
│   ├── widgets/
│   │   ├── countdown_card.dart            # Prominent countdown card with progress bar
│   │   ├── quote_card.dart                # Quote of the day widget
│   │   ├── subject_card.dart              # Subject grid item
│   │   ├── note_tile.dart                 # Note list item with bookmark toggle
│   │   ├── paper_tile.dart                # Paper item with PDF & marking scheme buttons
│   │   └── custom_search_bar.dart         # Search input widget
│   └── screens/
│       ├── main_nav_screen.dart           # Bottom navigation shell
│       ├── home_screen.dart               # Main student dashboard
│       ├── subject_detail_screen.dart     # Subject detail with dual Notes/Papers tabs
│       ├── note_view_screen.dart          # Rich note reader with text-size controls
│       ├── pdf_viewer_screen.dart         # PDF reader using flutter_pdfview
│       ├── papers_list_screen.dart        # Past & model papers hub with filters
│       ├── bookmarks_screen.dart          # Saved revision vault
│       ├── search_screen.dart             # Live keyword search screen
│       └── settings_screen.dart           # Dark mode, cache clear & exam date setup
├── firestore.rules                        # Cloud Firestore security rules
├── storage.rules                          # Firebase Storage security rules
├── sample_data.json                       # Seed data for Firestore
└── pubspec.yaml                           # Flutter dependencies
```

---

## 🗄️ Firebase Firestore Schema

### 1. `subjects` Collection
| Field | Type | Description |
|---|---|---|
| `id` | `String` | Unique ID (e.g., `'maths'`, `'science'`) |
| `name` | `String` | English Name (e.g., `'Mathematics'`) |
| `sinhalaName` | `String` | Sinhala Name (e.g., `'ගණිතය'`) |
| `tamilName` | `String` | Tamil Name (e.g., `'கணிதம்'`) |
| `category` | `String` | `'Core'`, `'Basket 1'`, `'Basket 2'`, etc. |
| `iconCodePoint`| `int` | Material icon code point |
| `colorHex` | `String` | Hex color code (e.g., `'#2563EB'`) |
| `totalUnits` | `int` | Number of syllabus units |
| `totalPapers`| `int` | Available past papers |

### 2. `notes` Collection
| Field | Type | Description |
|---|---|---|
| `id` | `String` | Note identifier |
| `subjectId` | `String` | Foreign key referencing `subjects` |
| `subjectName` | `String` | Denormalized subject name |
| `unitNumber` | `int` | Syllabus unit (e.g., `12`) |
| `title` | `String` | Note title (e.g., `'Quadratic Equations'`) |
| `summary` | `String` | Quick 2-line summary |
| `keyPoints` | `Array<String>` | High-yield exam bullet points / formulas |
| `contentBody` | `String` | Markdown / formatted text content |
| `pdfUrl` | `String?` | Optional Firebase Storage link to PDF note |
| `grade` | `int` | `10` or `11` |
| `readTimeMinutes` | `int` | Estimated reading duration in minutes |

### 3. `papers` Collection
| Field | Type | Description |
|---|---|---|
| `id` | `String` | Paper identifier |
| `subjectId` | `String` | Subject key (e.g., `'maths'`) |
| `title` | `String` | Title (e.g., `'2024 G.C.E. O/L Mathematics'`) |
| `year` | `int` | Examination year (e.g., `2024`) |
| `type` | `String` | `'Past Paper'` or `'Model Paper'` |
| `medium` | `String` | `'Sinhala'`, `'English'`, `'Tamil'` |
| `pdfUrl` | `String` | Firebase Storage download URL for Paper PDF |
| `markingSchemePdfUrl` | `String?` | URL for the Marking Scheme / Answers PDF |
| `fileSize` | `String` | File size display string (e.g., `'2.4 MB'`) |

---

## 🚀 Step-by-Step Setup & Run Guide

### Step 1: Install Flutter (if not yet on your PATH)
1. Download the Flutter SDK for Windows from [https://flutter.dev](https://flutter.dev).
2. Extract to `C:\src\flutter` and add `C:\src\flutter\bin` to your System Environment `PATH`.
3. Verify in PowerShell:
   ```powershell
   flutter doctor
   ```

### Step 2: Install App Dependencies
In this directory (`c:\Users\MCM\Desktop\OL Lesson`), run:
```powershell
flutter pub get
```

### Step 3: Run Immediately with Built-in Offline Syllabus Data
The app includes a built-in `MockDataService`. If Firebase is not yet connected, the app **starts immediately** with full functionality, sample Sri Lankan O/L subjects, notes, and past papers!
```powershell
flutter run
```

---

## ☁️ Connecting to Firebase

1. **Install FlutterFire CLI**:
   ```powershell
   dart pub global activate flutterfire_cli
   ```
2. **Login to Firebase**:
   ```powershell
   firebase login
   ```
3. **Configure your Flutter project**:
   ```powershell
   flutterfire configure
   ```
   This will automatically register your Android and iOS apps in your Firebase console and generate `lib/firebase_options.dart`.

4. **Android Configuration**:
   - Ensure `google-services.json` is placed in `android/app/google-services.json`.
   - The permissions (`INTERNET`, `ACCESS_NETWORK_STATE`) are already configured in `android/app/src/main/AndroidManifest.xml`.

5. **Deploy Security Rules**:
   ```powershell
   firebase deploy --only firestore:rules,storage:rules
   ```

6. **Upload Sample Data to Firestore**:
   - You can use the built-in helper method `FirestoreService().seedInitialDataIfEmpty()` or import `sample_data.json` directly from the Firebase Console.
