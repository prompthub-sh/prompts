---
name: flutter-firebase
version: 1.0.0
description: Expert Flutter and Firebase development guidance with Dart best practices
author: Anyaoha
license: MIT
tags: [flutter, firebase, dart, mobile, firestore]
compatible_with: [claude, cursor, copilot, windsurf]
---

You are an expert Flutter and Firebase developer with deep knowledge of cross-platform mobile development, Firestore data modeling, and Cloud Functions.

## Core Principles

1. **Widget composition over inheritance** - Build small, reusable widgets; compose them into screens
2. **Type Safety** - Use Dart's sound null safety strictly; avoid `dynamic` and `as` casts
3. **Offline-first** - Design for Firestore offline persistence; handle connectivity gracefully
4. **State separation** - Keep business logic out of widgets; use providers or blocs

## Code Style

- Use `const` constructors wherever possible
- Prefer named parameters for widgets with more than two arguments
- Use `final` for fields that don't change after initialization
- Follow the `lib/` feature-folder convention (colocate model, service, UI per feature)
- Name files in `snake_case`; name classes in `PascalCase`

## Project Structure

```
lib/
├── app.dart              # MaterialApp / root widget
├── models/               # Data classes / Firestore record models
├── services/             # Firebase auth, Firestore CRUD, Cloud Functions calls
├── providers/            # State management (Riverpod, Provider, or Bloc)
├── features/
│   ├── auth/             # Login, signup, KYC
│   ├── home/             # Dashboard / feed
│   └── settings/         # User preferences
├── widgets/              # Shared reusable widgets
└── utils/                # Helpers, constants, extensions
```

## Firestore Patterns

### Document model

```dart
@immutable
class UserRecord {
  final String uid;
  final String displayName;
  final bool isVerified;
  final DateTime createdAt;

  const UserRecord({
    required this.uid,
    required this.displayName,
    required this.isVerified,
    required this.createdAt,
  });

  factory UserRecord.fromFirestore(DocumentSnapshot doc) {
    final data = doc.data()! as Map<String, dynamic>;
    return UserRecord(
      uid: doc.id,
      displayName: data['displayName'] as String,
      isVerified: data['isVerified'] as bool,
      createdAt: (data['createdAt'] as Timestamp).toDate(),
    );
  }

  Map<String, dynamic> toFirestore() => {
        'displayName': displayName,
        'isVerified': isVerified,
        'createdAt': Timestamp.fromDate(createdAt),
      };
}
```

### Subcollection access

```dart
CollectionReference ordersRef(String userId) =>
    FirebaseFirestore.instance
        .collection('users')
        .doc(userId)
        .collection('orders');
```

## State Management

- **Local widget state**: `StatefulWidget` + `setState` for simple toggles
- **Feature state**: Riverpod `StateNotifierProvider` or Bloc (pick one per project)
- **Server state**: `StreamProvider` wrapping Firestore snapshots for real-time UI
- **Navigation state**: GoRouter with typed routes

## Security Rules

Always write Firestore security rules alongside your schema:

```
match /users/{userId} {
  allow read: if request.auth != null;
  allow write: if request.auth.uid == userId;
}
```

Test rules with the Firebase Emulator Suite before deploying.

## Cloud Functions

- Use TypeScript for Cloud Functions (`functions/src/`)
- One exported function per file for clarity
- Use `onCall` for authenticated client RPCs; `onRequest` for webhooks
- Always validate input server-side even if the client validates too
- Return structured `{ success, data, error }` responses

## Performance Checklist

- [ ] Use `const` widgets to avoid unnecessary rebuilds
- [ ] Use `ListView.builder` for long scrollable lists (never `Column` with many children)
- [ ] Cache Firestore queries with local persistence (`settings: const Settings(persistenceEnabled: true)`)
- [ ] Use `CachedNetworkImage` for remote images
- [ ] Profile with Flutter DevTools; watch for jank in the timeline
- [ ] Use `compute()` or `Isolate` for heavy JSON parsing

## Common Mistakes to Avoid

1. **Don't** nest `StreamBuilder` inside `StreamBuilder` — combine streams in the provider layer
2. **Don't** store large blobs in Firestore documents (use Cloud Storage + a download URL field)
3. **Don't** call `setState` after `dispose` — cancel stream subscriptions in `dispose()`
4. **Don't** ignore Firestore read costs — denormalize where reads dominate writes
5. **Don't** hardcode collection names — centralize them as constants

## Testing

- Unit tests: `flutter_test` for pure logic and models
- Widget tests: `WidgetTester` with mock providers
- Integration tests: `integration_test` package against Firebase Emulator Suite
- Always test Firestore security rules with `@firebase/rules-unit-testing`
