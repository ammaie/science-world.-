# عالم العلوم | Science World — المرحلة 1 (تطبيق الطلاب)

## المتطلبات
- Flutter مستقر (3.22 أو أحدث) وAndroid SDK وJDK 17.
- حساب Supabase (للمصادقة والاشتراك).

## التشغيل
```bash
cd science_world
flutter create --platforms=android --org com.scienceworld .   # يولّد مجلد android دون مسّ lib/
flutter pub get
flutter test
flutter run --dart-define=SUPABASE_URL=https://xxx.supabase.co --dart-define=SUPABASE_ANON_KEY=<anon-key>
flutter build apk --release --dart-define=SUPABASE_URL=... --dart-define=SUPABASE_ANON_KEY=...
```
أضف في `android/app/src/main/AndroidManifest.xml` داخل `<manifest>`:
`<queries><package android:name="com.whatsapp"/></queries>` و`<uses-permission android:name="android.permission.INTERNET"/>` (للإصدار النهائي).

## إعداد الخادم (Supabase)
1. أنشئ مشروعًا، ثم نفّذ `supabase/schema.sql` في SQL Editor.
2. Authentication ← Email: فعّل «Confirm email»، وعدّل قالب البريد ليحتوي `{{ .Token }}` (رمز OTP).
3. استخدم مفتاح **anon** فقط داخل التطبيق؛ لا تضع service-role في أي مكان بالتطبيق.
4. أكواد التفعيل: أنشئها من لوحة الإدارة لاحقًا أو من SQL (خزّن `sha256` للكود فقط).

## هيكل المجلدات
`lib/core` (ثيم، توجيه، إعداد) · `lib/data` (نماذج، SQLite، إعدادات) · `lib/features/*` (واجهات ومنطق كل ميزة) · `supabase/` · `test/`.

## ملاحظات
- لم يُضمَّن خط عربي؛ أضف Cairo أو Tajawal ضمن `assets/fonts` وصرّح به في `pubspec.yaml`.
- المؤثرات الصوتية مولّدة برمجيًا (أصلية) في `assets/sounds`.
- أسئلة العينة تظهر في وضع debug فقط وموسومة «عينة تقنية».

## البناء على GitHub (دون تثبيت Flutter محليًا)
1. ارفع كل محتويات هذا المجلد إلى جذر مستودع `science-world` (بما فيها المجلد المخفي `.github`).
2. Settings ← Secrets and variables ← Actions: أضف `SUPABASE_URL` و`SUPABASE_ANON_KEY`.
3. تبويب Actions ← Build APK ← Run workflow. عند النجاح نزّل `science-world-apk` من Artifacts.
