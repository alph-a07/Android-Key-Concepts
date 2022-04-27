# Contents
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#date-picker">Date Picker</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#on-click-animations">On click animations</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#activiry-switching-animations">Activity switching animations</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#loading-a-gif-as-animation">Loading a GIF as animation</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#google-signin-firebase">Google SignIn Firebase</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#hideview-password-in-textview">Hide/View password in TextView</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#dividing-two-views-in-equal-halves-in-relative-layout">Dividing two views in equal halves in relative layout</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#only-emoji-selector">Convert String to Editable</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#color-terminology">Color terminology</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#custom-toolbar-and-menu">Custom toolbar and menu in a layout</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#intent-types">Intent Types</a>
- <a href="https://github.com/alph-a07/Android-Key-Concepts/blob/main/README.md#google-sms-retriever-api">Google SMS Retriever API</a>

# Date Picker
#### Create fragment for dialog popup
~~~kotlin
        // Fragment required to pop dialog
        // extends -- DialogFragment (ofCourse)
        //         -- DatePickerDialog.OnDateSetListener (to specifically open DatePicker)
        class DatePickerFragment : DialogFragment(), DatePickerDialog.OnDateSetListener {

            // current date
            val c = Calendar.getInstance()
            val year = c.get(Calendar.YEAR)
            val month = c.get(Calendar.MONTH)
            val day = c.get(Calendar.DAY_OF_MONTH)

            // dialog pop up
            override fun onCreateDialog(savedInstanceState: Bundle?): Dialog {

                // Create a new instance of DatePickerDialog and return it
                val datePickerDialog = DatePickerDialog(requireContext(), this, year, month, day)

                datePickerDialog.show() // show to avoid NPE

                // color of positive response text
                datePickerDialog.getButton(DatePickerDialog.BUTTON_POSITIVE)?.setTextColor(
                    ContextCompat.getColor(
                        this@PersonalInfoActivity,
                        R.color.mustard
                    )
                )

                // color of negative response text
                datePickerDialog.getButton(DatePickerDialog.BUTTON_NEGATIVE)?.setTextColor(
                    ContextCompat.getColor(
                        this@PersonalInfoActivity,
                        R.color.hint_color
                    )
                )

                return datePickerDialog
            }

            // to decide what to do with selected date
            override fun onDateSet(view: DatePicker, year: Int, month: Int, day: Int) {

                // add 0 as prefix if date is single digit
                findViewById<TextView>(R.id.birthdate_date).text =
                    if (day / 10 > 0) day.toString() else "0$day"

                // add 0 if month is single digit
                // January is considered as 0th month so adjust accordingly
                findViewById<TextView>(R.id.birthdate_month).text =
                    if ((month + 1) / 10 > 0) day.toString() else "0" + (month + 1)

                findViewById<TextView>(R.id.birthdate_year).text = year.toString()
            }
        }
~~~

#### Show the fragment
```kotlin
val newFragment = DatePickerFragment() // instance of dialog fragment
newFragment.show(supportFragmentManager, "datePicker") // show fragment
```

#### Create custom style
```xml
<style name="datePickerTheme" parent="ThemeOverlay.AppCompat.Dialog">
        <item name="android:datePickerMode">spinner</item>   <!--to show popup as scrolling spinner-->
        <!--<item name="android:datePickerMode">calendar</item>  - to show popup as calender-->
</style>
```

#### Overriding datePickerStyle
```xml
<item name="android:datePickerStyle">@style/datePickerTheme</item>
```

#### Customizations for calender mode
![image](https://user-images.githubusercontent.com/83648189/164991579-88283104-ff46-4c14-b4a9-28f87f5e07d8.png)

# On click animations

#### Underline link animation to textView
```xml
<!--Store strings which require link underline formatting.-->
<string name="github"><a href="https://github.com/alph-a07/Android-App-Development/tree/master/ChatBox">Github</a></string>
```

#### Basic round shaped aniamtion
```xml
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android" android:color="#80A3A3A3" android:radius="23dp">
</ripple>
```
#### Custom shape ripple animation
```xml
<?xml version="1.0" encoding="utf-8"?>
<!--specify color of ripple animation-->
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="#DADADA">

    <!--Specifying shape and color of background-->
    <item android:id="@android:id/background">
        <shape>
            <solid android:color="#CCCAEFED" />
            <corners android:radius="20dp" />
        </shape>
    </item>
</ripple>
```

#### Or if you don't want to use custom background
```xml
android:background="?android:attr/selectableItemBackground"
```
or
```xml
android:background="?android:attr/selectableItemBackground"
```
# Activiry switching animations
Animation Resource - push_down_in.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
  <translate android:fromYDelta="-100%p" android:toYDelta="0" android:duration="5000"/>
  <alpha android:fromAlpha="0.0" android:toAlpha="1.0" android:duration="5000"/>
</set>
```

Animation Resource - push_down_out.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
  <translate android:fromYDelta="0" android:toYDelta="100%p" android:duration="5000"/>
  <alpha android:fromAlpha="1.0" android:toAlpha="0.0" android:duration="5000"/>
</set>
```

Animation Resource - push_up_in.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
  <translate android:fromYDelta="100%p" android:toYDelta="0" android:duration="5000"/>
  <alpha android:fromAlpha="0.0" android:toAlpha="1.0" android:duration="5000"/>
</set>
```

Animation Resource - push_up_out.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
  <translate android:fromYDelta="0" android:toYDelta="-100%p" android:duration="5000"/>
  <alpha android:fromAlpha="1.0" android:toAlpha="0.0" android:duration="5000"/>
</set>
```

Animation Resource - slide_in_left.xml
```xml
<?xml version="1.0"encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
        android:shareInterpolator="false">
        <translate android:duration="5000" android:fromXDelta="-100%" android:toXDelta="0%"/>
        <alpha android:duration="5000" android:fromAlpha="0.0" android:toAlpha="1.0"/>
</set>
```

Animation Resource - slide_in_right.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
        android:shareInterpolator="false">
        <translate android:duration="5000" android:fromXDelta="100%" android:toXDelta="0%"/>
        <alpha android:duration="5000" android:fromAlpha="0.0" android:toAlpha="1.0"/>
</set>
```

Animation Resource - slide_out_left.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
        android:shareInterpolator="false">
        <translate android:duration="5000" android:fromXDelta="0%" android:toXDelta="-100%"/>
        <alpha android:duration="5000" android:fromAlpha="1.0" android:toAlpha="0.0"/>
</set>
```

Animation Resource - slide_out_right.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android"
        android:shareInterpolator="false">
        <translate android:duration="5000" android:fromXDelta="0%" android:toXDelta="100%"/>
        <alpha android:duration="5000" android:fromAlpha="1.0" android:toAlpha="0.0"/>
</set>
```
Note: These animations can also be found in-built in R.anim resource directory.

Add this after startActivity(intent) call
```kotlin
overridePendingTransition(ENTRY_ANIMATION, EXIT_ANIMATION)
```

# Loading a GIF as animation
Break GIF into frames from <a href="https://ezgif.com/">here</a> and add frames to drawable resource directory.

```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list android:id="@+id/selected"
    android:oneshot="true" 
    xmlns:android="http://schemas.android.com/apk/res/android"> <!--true - only played once-->
    <item android:drawable="@drawable/welcome_subtitle1" android:duration="30" />
    <item android:drawable="@drawable/welcome_subtitle2" android:duration="30" />
    <item android:drawable="@drawable/welcome_subtitle3" android:duration="30" />
	.
	.
	.
    <item android:drawable="@drawable/welcome_subtitle91" android:duration="30" />
</animation-list>
```

```kotlin
// Load the ImageView that will host the animation and
// set its background to our AnimationDrawable XML resource.
val img: ImageView = findViewById<View>(R.id.ANIMATION_LIST) as ImageView
img.setBackgroundResource(R.drawable.ANIMATION_LIST)

// Get the background, which has been compiled to an AnimationDrawable object.
val frameAnimation = img.background as AnimationDrawable

// Start the animation (looped playback by default).
frameAnimation.start()
```

# Google SignIn Firebase

```kotlin
//region --> GOOGLE LOGIN CODE <--

        // helps open email access popup
        val gso =
            GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestIdToken(getString(R.string.client_id)) // displays app's name and logo in popup
                .requestEmail()
                .build()

        findViewById<ImageView>(R.id.login_google).setOnClickListener {
            val googleSignInClient = GoogleSignIn.getClient(this, gso)
            googleSignInClient.signOut() // clears cookies about last login and provides all email options each time
            startActivityForResult(googleSignInClient.signInIntent, 1)
        }
        //endregion
```

```kotlin
// must override to capture results from googleSignInClient
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        callbackManager.onActivityResult(requestCode, resultCode, data)
        super.onActivityResult(requestCode, resultCode, data)

        if (requestCode == 1) {
            // task = account info
            val task = GoogleSignIn.getSignedInAccountFromIntent(data)
            try {
                val account = task.getResult(ApiException::class.java)!!

                firebaseAuthWithGoogle(account.idToken!!)
            } catch (e: ApiException) {
                Toast.makeText(this, "SignIn failed,try again", Toast.LENGTH_SHORT).show()
            }
        }
    }
```

```kotlin
private fun firebaseAuthWithGoogle(idToken: String) {

        // user credentials
        val credential = GoogleAuthProvider.getCredential(idToken, null)

        auth.signInWithCredential(credential)
            .addOnCompleteListener(this) { task ->
                // Google signIn successful
                if (task.isSuccessful) {
                    val user = auth.currentUser
                    val model = User()
                    model.email = user?.email.toString()
                    model.userName = user?.displayName.toString()
                    myRef.child(auth.uid.toString())
                        .setValue(model)

                    updateUI(user)
                }
                // Google signIn failed
                else {
                    Toast.makeText(this, "Sign in failed.", Toast.LENGTH_SHORT).show()
                }
            }
    }
```

# Hide/View password in TextView
Example of hide/view password implementation in two textviews simultaneously
```kotlin
    @SuppressLint("ClickableViewAccessibility")
    // function to handle hide/show operations of password
    // operated on both edittext simultaneously
    private fun hideViewPassword(password1: EditText, password2: EditText) {

        // onTouch Lambda
        password1.setOnTouchListener { _, event ->

            // ACTION_DOWN is for the first finger that touches the screen. This starts the gesture. The pointer data for this finger is always at index 0 in the MotionEvent.
            // ACTION_POINTER_DOWN is for extra fingers that enter the screen beyond the first. The pointer data for this finger is at the index returned by getActionIndex().
            // ACTION_POINTER_UP is sent when a finger leaves the screen but at least one finger is still touching it. The last data sample about the finger that went up is at the index returned by getActionIndex().
            // ACTION_UP is sent when the last finger leaves the screen. The last data sample about the finger that went up is at index 0. This ends the gesture.
            // ACTION_CANCEL means the entire gesture was aborted for some reason. This ends the gesture.
            if (event!!.action == MotionEvent.ACTION_DOWN) {

                // event.x => Co-ordinate of the touch event w.r.t parent layout
                // event.rawX => Co-ordinate of the touch event w.r.t. screen
                // password.right => Co-ordinate of right end of editText
                // password1.compoundDrawables[2].bounds.width() => width of drawable right
                // password1.right - password1.compoundDrawables[2].bounds.width()) => Co-ordinate of starting point of drawable right
                // Perform action only if drawable right is touched
                if (event.rawX >= (password1.right - password1.compoundDrawables[2].bounds.width())) {

                    // CASE 1 : Password is hidden
                    if (password1.tag == "hidden" && password2.tag == "hidden") {

                        // Make password visible
                        password1.transformationMethod = null  // no transformation on text
                        password2.transformationMethod = null  // visible as it is

                        // Change icons
                        password1.setCompoundDrawablesRelativeWithIntrinsicBounds(
                            0,
                            0,
                            R.drawable.hide,
                            0
                        )
                        password2.setCompoundDrawablesRelativeWithIntrinsicBounds(
                            0,
                            0,
                            R.drawable.hide,
                            0
                        )

                        // Tag as visible
                        password1.tag = "visible"
                        password2.tag = "visible"
                        return@setOnTouchListener true
                    }

                    // CASE 2 : Password is visible
                    if (password1.tag == "visible" && password2.tag == "visible") {

                        // Make password hidden
                        password1.transformationMethod =
                            PasswordTransformationMethod()  // transformed text
                        password2.transformationMethod =
                            PasswordTransformationMethod()  // visible as dots

                        // Change icons
                        password1.setCompoundDrawablesRelativeWithIntrinsicBounds(
                            0,
                            0,
                            R.drawable.view,
                            0
                        )
                        password2.setCompoundDrawablesRelativeWithIntrinsicBounds(
                            0,
                            0,
                            R.drawable.view,
                            0
                        )

                        // Tag as hidden
                        password1.tag = "hidden"
                        password2.tag = "hidden"
                        return@setOnTouchListener true
                    }
                }
            }
            return@setOnTouchListener false
        }
    }
```

# Dividing two views in equal halves in relative layout

```xml
<RelativeLayout 
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    
    <View android:id="@+id/view"
        android:layout_width="0dp"
        android:layout_height="0dp" 
        android:layout_centerHorizontal="true"/>
        
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignRight="@id/view"
        android:layout_alignParentStart="true"
        android:text="Left"/> 
        
    <Button 
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@id/view"
        android:layout_alignParentEnd="true"
        android:text="Right"/>
</RelativeLayout>
```

# Convert String to Editable
This will be used when programmatically changin text of an EditText
```xml
val editable: Editable = SpannableStringBuilder("Pass a string here")
```

# Only Emoji Selector

Library used : https://github.com/vanniktech/Emoji <br>
Implemented at : https://github.com/alph-a07/ChatBox/tree/master/app/src/main/java/com/example/chatbox

XML Code
```xml
<com.vanniktech.emoji.EmojiEditText
        android:id="@+id/emojiEditText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"         
        android:cursorVisible="false"
        android:gravity="center"
        android:hint="👤" />
```

Code in activity
```kotlin
	 val emoji = findViewById(R.id.emojiEditText)  // EditText from library required to put emoji in
        val rootView = findViewById<View>(R.id.rootView)  // rootView required to build emoji keyboard popup
        val emojiPopup = EmojiPopup.Builder.fromRootView(rootView).build(emoji)  // initialising emojiPopup
        
        emoji.filters = arrayOf(MaximalNumberOfEmojisInputFilter(1))  // only one emoji allowed
        emoji.disableKeyboardInput(emojiPopup)  // disabling text input keyboard

        emoji.setOnClickListener {
            emoji.setSelection(1)  // sets cursor at the end of emoji if exists any
            emojiPopup.toggle()  // Toggles from device default keyboard to emoji keyboard
        }
```

Add this to manifest in corresponding activity
```xml
android:windowSoftInputMode="adjustResize"  <!---Adjusts the keyboard size automaticallly-->  
```

# Color Terminology

• colorPrimary: The primary brand color of your app, used most predominantly in theming <br>
• colorPrimaryVariant: A lighter/darker variant of your primary brand color, used sparingly in theming<br>
• colorOnPrimary: The color used for elements displayed on top of your primary colors (eg. Text and icons, often white or semi-transparent black depending on accessibility)<br>
• colorSecondary: The secondary brand color of your app, used mostly as an accent for certain widgets that need to stand out<br>
• colorSecondaryVariant: A lighter/darker variant of your secondary brand color, used sparingly in theming<br>
• colorOnSecondary: The color used for elements displayed on top of your secondary colors<br>
• colorError: The color used for errors (often a shade of red)<br>
• colorOnError: The color used for elements displayed on top of your error color<br>
• colorSurface: The color used for surfaces (i.e. Material “sheets”)<br>
• colorOnSurface: The color used for elements displayed on top of your surface color<br>
• android:colorBackground: The color behind all other screen content<br>
• colorOnBackground: The color used for elements displayed on top of your background color

# Custom toolbar and menu

Add toolbar in the XML file
```xml
<androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:background="@color/black"
                app:title="The Kitchen"
                android:minHeight="?attr/actionBarSize"
                android:popupTheme="@style/Theme.AppCompat.Light"
                app:layout_constraintEnd_toEndOf="parent"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toTopOf="@+id/linearLayout3"/>
```
Set the toolbar as actionbar
```xml
setSupportActionBar(toolbar as androidx.appcompat.widget.Toolbar)
```

Set theme style to NoActionBar
```xml
<style name="Theme.FoodApp"parent="Theme.MaterialComponents.DayNight.NoActionBar">
```

Create a menu file in drawable menu directory
```xml
<?xml version="1.0"encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

        <item
                android:id="@+id/i1"
                android:title="@string/my_account"/>
        <item
                android:id="@+id/i2"
                android:title="@string/favourites"/>
        <item
                android:id="@+id/i3"
                android:title="@string/categories"/>
        <item
                android:id="@+id/i4"
                android:title="@string/settings"/>
        <item
                android:id="@+id/i5"
                android:title="@string/help"/>
</menu>
```
Inflating menu to toolbar
```kotlin
        override fun onCreateOptionsMenu(menu:Menu?):Boolean {
                menuInflater.inflate(R.menu.toolbar_menu,menu)
                return true
        }

        override fun onOptionsItemSelected(item:MenuItem):Boolean {
                when(item.itemId){
                        // Code to performed when menuItem clicked

                        R.id.i1->Toast.makeText(this,"broadcasting",Toast.LENGTH_SHORT).show()
                        R.id.i2->Toast.makeText(this,"set items",Toast.LENGTH_SHORT).show()
                        R.id.i3->Toast.makeText(this,"create new group",Toast.LENGTH_SHORT).show()
                        else->{
                                Toast.makeText(this,"n",Toast.LENGTH_SHORT).show()
                        }
                }
                return true
        }
}
```

# Intent Types

	• image/jpeg
	• audio/mpeg4-generic
	• text/html
	• audio/mpeg
	• audio/aac
	• audio/wav
	• audio/ogg
	• audio/midi
	• audio/x-ms-wma
	• video/mp4
	• video/x-msvideo
	• video/x-ms-wmv
	• image/png
	• image/jpeg
	• image/gif
	• .xml ->text/xml
	• .txt -> text/plain
	• .cfg -> text/plain
	• .csv -> text/plain
	• .conf -> text/plain
	• .rc -> text/plain
	• .htm -> text/html
	• .html -> text/html
	• .pdf -> application/pdf
	• .apk -> application/vnd.android.package-archive


