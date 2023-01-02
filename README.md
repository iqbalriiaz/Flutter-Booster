# Api-Helper
The repository contains code snippet of API implementation in flutter.


 
## 1. install http package and import as http:

[http package](https://pub.dev/packages/http)

## 2. make request to the API ### Custom Http

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
