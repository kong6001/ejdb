# EJDB2 Flutter integration

Embeddable JSON Database engine http://ejdb.org Dart binding.

See https://github.com/Softmotions/ejdb/blob/master/README.md

For API usage examples take a look into [/example](https://github.com/Softmotions/ejdb/tree/master/src/bindings/ejdb2_flutter/example)
application.

## Example

pubspec.yaml

```yaml
dependencies:
  ejdb2_flutter:
```

```dart
import 'package:ejdb2_flutter/ejdb2_flutter.dart';

var db = await EJDB2Builder('test.db').open();

var id = await db.put('parrots', {'name': 'Bianca', 'age': 4});
  print('Bianca record: ${id}');

id = await db.put('parrots', {'name': 'Darko', 'age': 8});
print('Darko record: ${id}');

final q = db.createQuery('/[age > :age]', 'parrots');
await for (final doc in q.setInt('age', 3).execute()) {
  print('Found ${doc}');
}

await db.close();
```

## Supported platforms

- iOS
- Android

## iOS note

In order to build app with this binding you have
to add the following code into application `Podfile`:

```ruby
pre_install do |installer|
  # workaround for https://github.com/CocoaPods/CocoaPods/issues/3289
  Pod::Installer::Xcode::TargetValidator.send(:define_method, :verify_no_static_framework_transitive_dependencies) {}
end
```

## How build it manually

```sh
git clone https://github.com/Softmotions/ejdb.git

cd ./ejdb
mkdir ./build && cd build

cmake .. -DCMAKE_BUILD_TYPE=Release \
         -DANDROID_NDK_HOME=<path to Android NDK> \
         -DBUILD_FLUTTER_BINDING=ON
make

# Move generate to ejdb2 flutter pub package with example app
cd src/bindings/ejdb2_flutter/pub_publish
flutter pub get
cd ./example

# Start Android emulator
flutter run
```
