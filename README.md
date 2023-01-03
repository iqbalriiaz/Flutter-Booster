# Api-Helper

## 1. install http package and import as http:

[http package](https://pub.dev/packages/http)

## 2. make request to the API:

### 2.1 Custom Http
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
## 3. stream can be listened to using a StreamBuilder widget in Flutter:

```dart
  @override
  void initState() {
    fetchCategories();
    super.initState();
  }
```
```dart
Scaffold(
      appBar: AppBar(title: Text("Category")),
      body: Padding(
        padding: const EdgeInsets.all(10.0),
        child: StreamBuilder(
          stream: fetchCategories(),
          builder: (context, snapshot) {
            if (snapshot.hasError) {
              return Text('Error: ${snapshot.error}');
            }
            if (snapshot.hasData) {
              List<Datum> categories = snapshot.data ?? [];
              return GridView.builder(
                gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 2,
                    mainAxisSpacing: 5,
                    crossAxisSpacing: 10),
                itemCount: categories.length,
                itemBuilder: (context, index) {
                  return GestureDetector(
                    onTap: () {
                      Navigator.of(context).pushNamed('/subcategory',
                          arguments: categories[index]);
                    },
                    child: Card(
                      elevation: 10.0,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(10.0),
                      ),
                      child: Padding(
                        padding: const EdgeInsets.all(10.0),
                        child: Column(
                          children: [
                            Expanded(
                                flex: 4,
                                child: Image.network(
                                  "$imageUrl${categories[index].image}",
                                  fit: BoxFit.cover,
                                )),
                            Spacer(),
                            Expanded(
                                flex: 1,
                                child: Text(
                                  "${categories[index].name}",
                                  style: TextStyle(fontSize: 20),
                                )),
                          ],
                        ),
                      ),
                    ),
                  );
                },
              );
            }
            return Center(child: CircularProgressIndicator());
          },
        ),
      ),
    )
```

# Date-Time Picker

## Date Picker

Install intl package to format the Date-Time
[intl package](https://pub.dev/packages/intl)

``` dart
String? dob;

 void _selectedDate() async {
    final selectedDate = await showDatePicker(
        context: context,
        initialDate: DateTime.now(),
        firstDate: DateTime(1950),
        lastDate: DateTime.now());
    if (selectedDate != null) {
      setState(() {
        dob = DateFormat("dd/MM/yyyy").format(selectedDate);
        
      });
    }
  }
```
You can calculate your current age, next-birthday and many more using Age-Calculator package
[age-calculator package](https://pub.dev/packages/age_calculator/example)


``` dart
  void _selectedDate() async {
    final selectedDate = await showDatePicker(
        context: context,
        initialDate: DateTime.now(),
        firstDate: DateTime(1950),
        lastDate: DateTime.now());
    if (selectedDate != null) {
      setState(() {
        dob = DateFormat("dd/MM/yyyy").format(selectedDate);
        picked_birthday =
            AgeCalculator.age(selectedDate, today: DateTime.now());
        next_birthday = AgeCalculator.timeToNextBirthday(selectedDate,
            fromDate: DateTime.now());
      });
    }
  }
}

```
