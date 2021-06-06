# Flutter TextField Autocomplete

## A Flutter package for TextField Autocomplete

![pub package][version_badge]

## Usage

```

import 'package:flutter_textfield_autocomplete/flutter_textfield_autocomplete.dart';


List<String> _typesOfBreed = []; // here you can declare your list instance.

GlobalKey<TextFieldAutoCompleteState<String>> _textFieldAutoCompleteKey =
      new GlobalKey(); // just copy paste this declaration.

@override
  void initState() {
    super.initState();
    _getPetBreeds(); // init the method where your list is populated
  }


// get the data from api and populate your list.
// this is just an example method. Declare the method your own
  _getPetBreeds() async {
    try {
        final _petService = PetService();
        var result = await _petService.getBreeds();
          var breeds = json.decode(result.body);
          breeds['breeds'].forEach((breed) {
            setState(() {
              _typesOfBreed.add(breed['title']);
            });
          });
    } catch (e) {
      setState(() {
        _isLoading = false;
      });
      return e.toString();
    }
  }


// design the container section your own
Container(
          height: 58,
          decoration: BoxDecoration(
            borderRadius: BorderRadius.circular(4),
          ),
          child: TextFieldAutoComplete(
              decoration: InputDecoration(
                enabledBorder: new OutlineInputBorder(
                  borderSide: BorderSide(color: borderColor),
                ),
                focusedBorder: new OutlineInputBorder(
                  borderSide: BorderSide(color: borderColor),
                ),
                labelText:
                    "${getTranslated(context, 'pet_breed_app') ?? 'Pet breed'}",
                hintText:
                    "${getTranslated(context, 'pet_breed_app') ?? 'Pet breed'}",
                floatingLabelBehavior: FloatingLabelBehavior.always,
                hintStyle: TextStyle(
                  color: secondaryColorWithOpacity,
                  fontSize: hintTextSize,
                  fontFamily: 'Roboto',
                  fontWeight: FontWeight.w300,
                ),
              ),
              clearOnSubmit: false,
              controller: _breed,
              itemSubmitted: (String item) {
                print('selected breed $item');
                _breed!.text = item;
                print('selected breed 2 ${_breed!.text}');
              },
              key: _textFieldAutoCompleteKey,
              suggestions: _typesOfBreed,
              itemBuilder: (context, String item) {
                return Container(
                  padding: EdgeInsets.all(20),
                  child: Row(
                    children: [
                      Text(
                        item,
                        style: TextStyle(color: Colors.black),
                      )
                    ],
                  ),
                );
              },
              itemSorter: (String a, String b) {
                return a.compareTo(b);
              },
              itemFilter: (String item, query) {
                return item
                    .toLowerCase()
                    .startsWith(query.toLowerCase());
              }),
        ),

```

