# Api-Helper
The repository contains code snippet of API implementation in flutter.


 
## 1. install http package and import as http:

[http package](https://pub.dev/packages/http)

## 2. make request to the API

### Custom Http
``` dart
List<Datum> categoriesList = [];
String apiUrl = "someUrl";

Future fetchCategories() async {
  final response = await http.get(Uri.parse(categoryUrl));
  Datum datum;

  if (response.statusCode == 200) {
    var data = jsonDecode(response.body);
    for (var i in data["data"]) {
      datum = Datum.fromJson(i);
      categoriesList.add(datum);
    }
  } else {
    throw Exception('Failed to load categoriesList');
  }
  return categoriesList;
}
```
In the code snippet, the fetchCategories() function is now a generator function that uses the yield keyword to return a stream of lists of Datum objects.

The yield keyword is used to indicate that the categories list should be returned as part of the stream. Each time the generator is iterated over, it will execute the code inside the loop, adding a new Datum object to the categories list for each item in the data["data"] list. It will then yield the updated categories list, causing it to be emitted as a new event in the stream.

This means that whenever this generator function is called, it will return a stream that emits a new list of Datum objects each time the generator is iterated over. The stream can be listened to using a StreamBuilder widget in Flutter, for example, to perform some action each time a new event is emitted.


If you do not want to use a generator function, you can use a regular function and return the value using the return keyword instead.

```dart
Stream<List<Datum>> fetchCategories() async {
  final response = await http.get(Uri.parse(categoryUrl));
  Datum datum;

  if (response.statusCode == 200) {
    var data = jsonDecode(response.body);
    List<Datum> categories = [];
    for (var i in data["data"]) {
      datum = Datum.fromJson(i);
      categories.add(datum);
    }
    return Stream.value(categories);
  } else {
    throw Exception('Failed to load categories');
  }
}
```
