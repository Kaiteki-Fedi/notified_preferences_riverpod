# notified_preferences_riverpod

A simple package providing glue methods between [notified_preferences](https://pub.dev/packages/notified_preferences) and [flutter_riverpod](https://pub.dev/packages/flutter_riverpod).

## Usage

Use it like any other provider. Make sure you provide an instance of SharedPreferences on startup of your app.

```dart
final preferencesProvider = 
    Provider<SharedPreferences>((_) => throw UnimplementedError());

final hasSeenTutorialProvider =
    createSettingProvider(key: 'hasSeenTutorial', initialValue: false);

void main() {
    final preferences = await SharedPreferences.getInstance();

    runApp(
        ProviderScope(
            overrides: [
                preferencesProvider.overrideWith((_) => preferences),
            ],
            child: const App(),
        ),
    );
}

class App extends ConsumerWidget {
    Widget build(BuildContext context, WidgetRef ref) {
        final hasSeenTutorial = ref.watch(hasSeenTutorialProvider).value;

        if (!hasSeenTutorial) return Text("Hello!");

        return OutlinedButton(
            child: Text("Exit tutorial"),
            onPressed: () => ref.read(hasSeenTutorialProvider).value = true,
        );
    }
}
```

For more guidance, check out [notified_preference's page](https://pub.dev/packages/notified_preferences#getting-started).