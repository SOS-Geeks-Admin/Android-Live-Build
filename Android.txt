Application components
=======================

	1. Application components are the essential building blocks of the application, 
	2. This components are loosely coupled from the manifest file, 
	3. AndroidManifest.xml describes each component of the application and how they interact
	4. Basic Components : Activities,services,broadcastreciver,contentproviders,Notifications
	5. Additional components: Fragments,views,Layouts,resources and manifest.
	
	
 Activities
 
 1. Activity represents a single screen with an user interface.
 2. One app can contain any number of activities all activities must be registered in the androidmanifest.xml file
 3. To create an Activity we must create a class that Extends AppCompatActivity and overide onCreate() method
	Syntax: Class MyActivity Extends AppcompatActivity{ }
 4. Oncreate methods should be compulsary override.
 5. Example: Email App contains the 2 Text fields, to mention the to address and the description and a button called compose all this view are placed inside 1 activity.
 
 Fragments
 
 1. Fragment is a part of an activity , It is also an reusable portions of user interface
 2. Fragments can be added using XML or programaticaaly during runtime.
 3. By using XML (Adding Fragment in activity_main.xml)
		<fragment
		android:id="@+id/fragments"
		android:name="com.abhiandroid.fragmentexample.SimpleFragment"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:layout_marginTop="10dp" />
 4. To add fragment at runtime we must provide a containerview for the fragment in activity layout file in which you can insert the fragment
	    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/fragment_container/>
 5. Adding Fragment Programattically
	 FragmentManager fm = getSupportFragmentManager();
     FragmentTransaction ft = fm.beginTransaction();
     ft.add(R.id.fragment_container, f,null);
	 ft.addToBackStack(null); // To add fragment to backstack
     ft.commit();
	
 6. If we add fragment using xml we cannot perform add, remove or replace, if we need to perform this we need to add prgromatically
 7. Syntax to create Fragment is Create a class that extends Fragment and overide OnCreateView() the OncreateView() takes three parameters LayoutInfalter,container,Bundle, In OnCreateView we will inflate the layout
 
 Services
 
 1. Services are the long running process that are executed in background of an android device.
 2. Services do not require any user interface.
 3.	Services are not bound to the lifecycle of an Activity, it runs in the background even if the Activity or Application is Destroyed
 4. Their are 3 types of service 
	1 UnBoundService or Start Service
	2 BoundService and
	3 IntentService,
 5. The Disadvantage of services is , The service stops when we clear app from background Tasks(clear data)
 6. If we want to create a NonStop Services we need the Broadcast receiver , when service going to stop it restarts it again by sending Broadcast.
 7. Class Myservices extends Service { }
 8. Example: A services may play a music in the background when it is in different application, or it might fetch data from the server with out blocking the user interaction in activity like file download.
 
 
 Broadcast Receivers
 
 1. An android app which receives the message from another app or from the android system itself
 
	
	
	
	
	
Context
=======
A Context is a handle to the system; it provides services like resolving resources, obtaining access to databases and preferences, and so on.
An Android app has activities. Context is like a handle to the environment your application is currently running in.


Application Context: 
--------------------
This context is tied to the lifecycle of an application. The application context(getApplicationContext()) can be used where you need a context whose lifecycle is separate from the current context or when you are passing a context beyond the scope of an activity.

Example Use: If you have to create a singleton object for your application and that object needs a context, always pass the application context.


Activity Context
---------------
This context is available in an activity. This context is tied to the lifecycle of an activity. The activity context should be used when you are passing the context in the scope of an activity or you need the context whose lifecycle is attached to the current context.



There are two ways to implement MVVM in Android:

    Data Binding
    RXJava

ViewModel notifies the View when to show a Toast Message without keeping a reference of the View.

NOTE::
How is it possible to notify some class without having a reference of it?

It can be done in three different ways:

    Using Two Way Data Binding
    Using Live Data
    Using RxJava


Data Binding
------------
DataBinding layout is different than the normal layout , It starts with the root Tag <layout> and followed by the <data> Tag, the <data> tag contains the multiple <variable> tag within it to describes the property and and then followed by <view> tag and for each layout file ,
for example the layout filename is activity_main.xml so the corresponding generated class is ActivityMainBinding. this class holds all the bindings from the layout properties



Two Way Data Binding
------------------
Two-way Data Binding is a technique of binding your objects to your XML layouts such that the Object and the layout can both send data to each other.
In our case, the ViewModel can send data to the layout and also observe changes.
For this, we need a BindingAdapter and custom attribute defined in the XML.


@Bindable
---------
The Bindable annotation should be applied to any getter accessor method of an Observable class. Bindable will generate a field in the BR class to identify the field that has changed. 



