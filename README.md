# Smart Health App

A Flutter iOS app for AI-powered patient health monitoring.

## Getting Started

Smart Health connects Apple Watch wearable data, hospital clinical records, and Google Gemini AI in one place for patients. Patients can log in, sync their Apple Watch, view their clinical history, track medications, log symptoms, and chat with an AI health assistant.

## Features

Email-based login authenticated against the eHospital backend

Stores patient_id, username, and email using SharedPreferences

Apple HealthKit integration (iOS)
- Heart rate
- Steps
- Active energy burned
- Sleep

Automatic sync of wearable vitals to the eHospital backend

Clinical records from the hospital API
- ECG results
- Lab tests
- Diabetes and heart disease risk scores
- Stroke prediction
- Diagnosis history

AI health insights using Google Gemini

Conversational AI assistant with real patient data as context

Medication tracker with daily reset

Symptom logger with severity rating (1–5)

Health goals with progress bars for steps, sleep, and calories

BMI calculator

Week-over-week trend comparison with charts

Emergency SOS with one-tap 911 call and emergency contacts

Patient profile pulled from hospital records

## Tech Stack

Flutter (Dart)

Apple HealthKit

Google Gemini API

REST API (eHospital backend on AWS App Runner)

SharedPreferences

fl_chart

flutter_local_notifications

pdf + printing

Python (data pipeline)

## Project Structure

```
lib/
  Screens/
    login_screen.dart
    dashboard_screen.dart
    vitals_screen.dart
    vitals_history_screen.dart
    health_insights_screen.dart
    trend_comparison_screen.dart
    device_connection_screen.dart
    health_assistant_screen.dart
    medication_tracker_screen.dart
    symptom_logger_screen.dart
    health_goals_screen.dart
    bmi_calculator_screen.dart
    emergency_sos_screen.dart
    profile_screen.dart
    settings_screen.dart

  Services/
    e_hospital_auth_service.dart
    apple_health_service.dart

  config/
    api_config.dart  (gitignored — create manually)

  ui/
    app_theme.dart

scripts/
  data_pipeline.py
  requirements.txt

mvp.ipynb
pubspec.yaml
```

## Running the App

Install dependencies
```
flutter pub get
```

Create the config file

lib/config/api_config.dart is gitignored. Create it manually:
```dart
class ApiConfig {
  static const String baseUrl = 'https://aetab8pjmb.us-east-1.awsapprunner.com';
  static const String geminiApiKey = 'YOUR_GEMINI_API_KEY';
}
```

iOS Setup and Xcode Commands

Install CocoaPods
```
sudo gem install cocoapods
```

Install iOS pods
```
cd ios
pod install
cd ..
```

Open Xcode workspace
```
open ios/Runner.xcworkspace
```

In Xcode:
- Select your Team
- Ensure the Bundle Identifier is valid
- Enable HealthKit capability
- Ensure Deployment Target is iOS 14.0+

Run the app
```
flutter run
```

Note: HealthKit does not work in the simulator. Use a physical iPhone paired with an Apple Watch.

## Backend API

Base URL: https://aetab8pjmb.us-east-1.awsapprunner.com

GET /table/<table_name>?patient_id=<id>

Tables:
- users
- wearable_vitals
- vitals_history
- ecg
- lab_tests
- diabetes_analysis
- heart_disease_analysis
- stroke_prediction
- diagnosis

## Data Pipeline

Fetches all clinical tables and exports a merged CSV for analysis.
```
cd scripts
pip install -r requirements.txt
python data_pipeline.py
```

Output: Patient_20_Integrated_Data.csv

## Known Issues and Fixes

iOS build errors

Fixed by deleting:
```
ios/Pods/
ios/Podfile.lock
ios/build/
```
Then reinstalling pods.

HealthKit sync delay

A few seconds delay is expected. The app reads from HealthKit, uploads to the API, then fetches back.

Apple Health permissions

Handled with permission checks on launch. If denied, vitals will not sync.

## Future Improvements

Android support

Background HealthKit sync with push notifications

Encrypted local storage

Clinician-facing dashboard

Predictive health alerts

Medication reminders with AI guidance

## Disclaimer

This app is for informational purposes only. It is not a medical device and does not provide medical advice, diagnosis, or treatment. Always consult a qualified healthcare professional before making any health decisions.

