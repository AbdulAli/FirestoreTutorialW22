# 🔥 Firestore Tutorial 🔥
### Abdul Ali Bangash (University of Alberta, AB, Canada) - WINTER 2022 

* Caution ⚠️ : Before we get started, always remember, 
  *	Please don’t touch build.gradle files until this tutorial guide asks you to 🙂
  *	At the end, make sure that you have the right firestore dependency that you auto imported through Option+Return (MAC) or Ctrl+Enter (Windows). Yes, don’t copy and paste any firestore dependency, let Android Studio resolve it for you by itself.

# Let’s get started!
**Step 1:** 
* Clone [ListyCityLink](https://github.com/simpleParadox/CMPUT-301-CustomList.git) project: `git clone https://github.com/simpleParadox/CMPUT-301-CustomList.git` 
* Hint: If you are not familiar with “git clone”, please refer to Lab-04 material.

**Step 2:** 
* Go to your project and navigate to activity_main.xml. We will add two EditTexts
and a Button to add a new city and province to the list and concurrently store the city and province in the
Firestore. We create an additional LinearLayout to hold these three new UI components. Note that the
orientation is horizontal for the nested LinearLayout. This makes sure that the views are placed
beside each other.

This is what your activity_main.xml would look like: \

  <?xml version="1.0" encoding="utf-8"?> 
  <LinearLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:tools="http://schemas.android.com/tools"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:orientation="vertical"
  tools:context=".MainActivity">
  <LinearLayout
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:orientation="horizontal">
  <EditText
  android:id="@+id/add_city_field"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:ems="8">
  </EditText>
  <EditText
  android:id="@+id/add_province_edit_text"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:ems="7">
  </EditText>
  <Button
  android:id="@+id/add_city_button"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:text="Add City">
  </Button>
  </LinearLayout>
  <ListView
  android:id="@+id/city_list"
  android:layout_width="match_parent"
  android:layout_height="match_parent">
  </ListView>
  </LinearLayout>



After updating the code in activity_main.xml, you will get something as follows.
NOTE: You can customize the layouts in any way you want

 
As you can see, there are two new EditText views and a button to add a new city to the list.
We have completed updating the layout. Let’s write up the logic for the two EditTexts and the Button.

First, declare the following as class variables, in the Main Activity.
final String TAG = "Sample";
Button addCityButton;
final EditText addCityEditText;
final EditText addProvinceEditText;
FirebaseFirestore db;


And get the reference to the corresponding UI components from the xml file. iInside the ‘onCreate’ method, do:

addCityButton = findViewById(R.id.add_city_button);
addCityEditText = findViewById(R.id.add_city_field);
addProvinceEditText = findViewById(R.id.add_province_edit_text);



Make sure you have the cityDataList and the adapter set to their correct values as follows: (They were already there in the code)

// Get a reference to the ListView and create an object for the city list
cityList = findViewById(R.id.city_list);
cityDataList = new ArrayList<>();
// Set the adapter for the ListView to the CustomAdapter that we created in Lab 3
cityAdapter = new CustomList(this, cityDataList);
cityList.setAdapter(cityAdapter);



Let’s write the logic for the ‘addCityButton’ view which adds a new city to the list and also stores the data to firestore.

addCityButton.setOnClickListener( new View.OnClickListener() {
@Override
public void onClick(View view) {
}
});

The next step is to actually write something inside the overriden ‘onClick’ method so that we have some functionality.
First of all, let’s get the city name and province name from the EditText fields.

// Retrieving the city name and the province name from the EditText fields
final String cityName = addCityEditText.getText().toString();
final String provinceName = addProvinceEditText.getText().toString()

Then we will create a HashMap to store the data in the form of Key-value pairs.

HashMap<String, String> data = new HashMap<>();

The next step is to check whether the user has entered something in the field or not. (We don’t want an empty city or a city with no province. That will be undesirable).

if (cityName.length()>0 && provinceName.length()>0) {


}

If everything is okay, we add the data into the HashMap, so in the if block, we write:

