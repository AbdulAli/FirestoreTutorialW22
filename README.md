# 🔥 Firestore Tutorial 🔥
### Abdul Ali Bangash (University of Alberta, AB, Canada) - WINTER 2022 

* Caution ⚠️ : Before we get started, always remember, 
  *	Please don’t touch build.gradle files until this tutorial guide asks you to 🙂
  *	At the end, make sure that you have the right firestore dependency that you auto imported through Option+Return (MAC) or Ctrl+Enter (Windows). Yes, don’t copy and paste any firestore dependency, let Android Studio resolve it for you by itself.

# Let’s get started!
### Note: [Step 1](#step-1) and [Step 2](#step-2) is all about what we already know from previous labs. Firebase starts from [Step 3](#step-3).

### Step 1:
* Clone [ListyCityLink](https://github.com/simpleParadox/CMPUT-301-CustomList.git) project: `git clone https://github.com/simpleParadox/CMPUT-301-CustomList.git` 
* Hint: If you are not familiar with “git clone”, please refer to Lab-04 material.

### Step 2:
* Go to your project and navigate to activity_main.xml. We will add two EditTexts
and a Button to add a new city and province to the list and concurrently store the city and province in the
Firestore. We create an additional LinearLayout to hold these three new UI components. Note that the
orientation is horizontal for the nested LinearLayout. This makes sure that the views are placed
beside each other. This is what your activity_main.xml would look like: [link](https://github.com/AbdulAli/FirestoreTutorialW22/blob/main/activity_main.xml)

* After updating the code in activity_main.xml, you will get something as follows.
NOTE: You can customize the layouts in any way you want

  <img width="250" alt="Screen Shot 2022-02-07 at 1 08 04 PM" src="https://user-images.githubusercontent.com/6480345/152863616-ca92ffad-b09a-4ebb-805d-29c9b162640f.png">

As you can see, there are two new EditText views and a button to add a new city to the list.
We have completed updating the layout. Let’s write up the logic for the two EditTexts and the Button.

* First, declare the following as class variables, in the `Main Activity`.

```
  final String TAG = "Sample"; 
  Button addCityButton; 
  final EditText addCityEditText;
  final EditText addProvinceEditText;
  FirebaseFirestore db; 
```

* Get the reference to the corresponding UI components from the xml file. Inside the `onCreate` method, do:
```
addCityButton = findViewById(R.id.add_city_button);
addCityEditText = findViewById(R.id.add_city_field);
addProvinceEditText = findViewById(R.id.add_province_edit_text);
```

* Make sure you have the `cityDataList` and the `adapter` set to their correct values as follows: (They were already there in the code)
```
// Get a reference to the ListView and create an object for the city list
cityList = findViewById(R.id.city_list);
cityDataList = new ArrayList<>();
// Set the adapter for the ListView to the CustomAdapter that we created in Lab 3
cityAdapter = new CustomList(this, cityDataList);
cityList.setAdapter(cityAdapter);
```

* Write the logic for the `addCityButton` view which adds a new `city` to the list and also stores the data to firestore.
```
addCityButton.setOnClickListener( new View.OnClickListener() {
@Override
public void onClick(View view) {
}
});
```

* The next step is to actually write something inside the overriden `onClick` method so that we have some functionality. \
First of all, let’s get the city name and province name from the EditText fields.
```
// Retrieving the city name and the province name from the EditText fields
final String cityName = addCityEditText.getText().toString();
final String provinceName = addProvinceEditText.getText().toString()
```

* Then we will create a `HashMap` to store the data in the form of `Key-value pairs`.
```
HashMap<String, String> data = new HashMap<>();
```

* The next step is to check whether the user has entered something in the field or not. (We don’t want an empty `city` or a `city` with no `province`. That will be undesirable).
```
if (cityName.length()>0 && provinceName.length()>0) {

}
```

* If everything is okay, we add the data into the HashMap, so in the if block, we write:
```
// If there’s some data in the EditText field, then we create a new key-value pair.
data.put("Province Name", provinceName);
```

### Step 3: Creating Firebase Database

* Go to the [firebase console](https://console.firebase.google.com/), and click `Add Project`.
  * Give project the name you like, and click `Continue`.
  * Disable the toggle that says, enable Google Analytics (you can keep it if you like), and click `Continue`.
  * Wait for the project to be initialized .... and click `Continue` once done.
* You must have landed on your project page, and it says -> `Cloud Firestore`, click that. \
   <img width="502" alt="Screen Shot 2022-02-07 at 2 14 38 PM" src="https://user-images.githubusercontent.com/6480345/152873222-3cf5b651-8e76-4c5b-b18f-8b7aec8d4df6.png">
  * Click on `Create database`.
  * Click on `Start in test mode`. Because we want to allow everyone to write on this project. Don't chooose this optionfor your course project.
  * Click `Next`.
  * Go ahead with the default server-location. Click `Enable`.
  * This is your Firebase database in Cloud. From now onwards, we will store data in this database using our app.

  
### Step 4: Integrating Firebase Database with Local Application
* Go to the [firebase console](https://console.firebase.google.com/), and click on your project. On the landing page, you'll see something like -> "Get started by adding Firebase to your app". It would show:
   <img width="180" alt="Screen Shot 2022-02-07 at 1 35 14 PM" src="https://user-images.githubusercontent.com/6480345/152867690-1af8b00d-9f42-47d0-b69b-2baf807fb905.png">
  * Click on the Android icon as highlighted. 
  * In `Android package name`, write your package name. In your case: `com.example.simpleparadox.listycity`. Make sure this package name is the same as what you have in your project. See: \
    <img width="180" alt="Screen Shot 2022-02-07 at 1 38 58 PM" src="https://user-images.githubusercontent.com/6480345/152868246-6639376b-45ca-4df9-8bdb-5fa92b2bf360.png">
  * You can provide your `App nickname`, and leave the `Debug signing certificate SHA-1` option blank.
  * Click on `Register App`.
  * Download `google-services.json`. Drag this JSON file and place it where it indicates in this image below. \
    <img width="180" alt="Screen Shot 2022-02-07 at 1 46 16 PM" src="https://user-images.githubusercontent.com/6480345/152869342-bada1017-6926-44d8-b3e3-18e44003c58f.png">
  * This JSON file has all the security credentials that would help your app connect with google's firebase database that you created earlier.
  * But how will you app know that it has to read from this JSON? Don't worry! Follow the next steps.
  * Click `Next`.
  * ⚠️ ⚠️ YOU HAVE TWO `build.gradle` FILES! ⚠️ ⚠️ One `build.gradle` resides in `Project`, while the other `build.gradle` resides in `app:Module`. See: \
    <img width="180" alt="Screen Shot 2022-02-07 at 1 51 13 PM" src="https://user-images.githubusercontent.com/6480345/152869969-159d2364-85e7-40f0-aadd-42e2beb4224b.png">
  * Go to your project level `build.gradle`
    * Check that you have `google()` in `buildscript { repositories { ... `
    * Add `classpath 'com.google.gms:google-services:4.3.10'` in `dependencies {`
    * Also check that you have `google()` in `allprojects { repositories { ... `
  * Go to your app:Module level `build.gradle`
    * Add `apply plugin: 'com.google.gms.google-services'` under this line `apply plugin: 'com.android.application'` 
    * DO NOT MAKE ANY CHANGES IN `dependencies{..`, even if the firebase website tells you to. 🙂
  * Sync your project by clicking this elephant on the top right corner: <img width="53" alt="Screen Shot 2022-02-07 at 2 00 53 PM" src="https://user-images.githubusercontent.com/6480345/152871107-ded91116-9036-47c5-ac1b-c1f86f2df4ca.png">
  * Click `Next`
  * Click `Continue to console`
 * You are done creating a firebase project, and a database in it on cloud and also created a mapping (JSON file) between your code and the cloud database. Congratulations! 🎉 Now we are going to add stuff into this databaes using our app.
 * Let's get back to the code to make sure that whenever someone clicks `ADD CITY`, the city and province get added to the `database` in cloud.


