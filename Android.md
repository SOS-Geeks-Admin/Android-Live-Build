 Android Architecture
 ==================================
 1. Linux Kernal
 2. Hardware Abstraction Layer
 3. Native C/C++ Library along with ART
 4. Java API framework
 5. System Apps

 Application components
 =======================

 1. Application components are the essential building blocks of the application, 
 2. This components are loosely coupled from the manifest file, 
 3. AndroidManifest.xml describes each component of the application and how they interact
 4. Basic Components : Activities,services,broadcastreciver,contentproviders,Notifications
 5. Additional components: Fragments,views,Layouts,resources and manifest.
	
 Activities :
 ============
 
 1. Activity represents a single screen with an user interface.
 2. One app can contain any number of activities all activities must be registered in the androidmanifest.xml file
 3. To create an Activity we must create a class that Extends AppCompatActivity and overide onCreate() method
	Syntax: Class MyActivity Extends AppcompatActivity{ }
 4. Oncreate methods should be compulsary override.
 5. Example: Email App contains the 2 Text fields, to mention the to address and the description and a button called compose all this view are placed inside 1 activity.
 
 Fragments :
 ===========
 
 1. Fragment is a part of an activity , It is also an reusable portions of user interface
 2. Fragments can be added in 2 ways using XML or programaticaaly during runtime.
 3. By using XML (Adding Fragment in activity_main.xml)
		<fragment
		android:id="@+id/fragments"
		android:name="com.aravind.fragmentexample.SimpleFragment"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:layout_marginTop="10dp" />
 4. To add fragment at runtime we must provide a containerview for the fragment in activity layout file in which you can insert the fragment
	    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/fragment_container/>
 5. Adding Fragment Programattically
 	 MyFragment f = new MyFragment();
	 FragmentManager fm = getSupportFragmentManager();
     FragmentTransaction ft = fm.beginTransaction();
     ft.add(R.id.fragment_container, f,null);
	 ft.addToBackStack(null); // To add fragment to backstack
     ft.commit();
 6. If we add fragment using xml we cannot perform add, remove or replace, if we need to perform this we need to add prgromatically
 7. Syntax to create Fragment is Create a class that extends Fragment and overide OnCreateView() the OncreateView() takes three parameters LayoutInfalter,container,Bundle, In OnCreateView we will inflate the layout
 
 Services :
 ===========

 1. Services are the long running process that are executed in background of an android device.
 2. Services do not require any user interface.
 3. Service is not bound to the activity lifecycle, it runs in the background even if the Activity or Application is Destroyed
 4. Their are 3 types of service 
	1 UnBoundService or Start Service
	2 BoundService and
	3 IntentService,
 5. The Disadvantage of services is , The service stops when we clear app from background Tasks(clear data)
 6. If we want to create a NonStop Services we need the Broadcast receiver , when service going to stop it restarts it again by sending Broadcast.
 7. Class Myservices extends Service { }
 8. Example: A services may play a music in the background when it is in different application, or it might fetch data from the server with out blocking the user interaction in activity like file download.
 
 UnBound Service :
 =================
 
 1. Unbound Service also called as StartService, An application component, such as an activity, starts it by calling startService(). Once started, a service can run in the background indefinitely, even if the component that started it is destroyed.
 2. Unbound Service gets starts by calling startService().
 3. Unbound Service is stopped or destroyed explicitly  by calling stopService().
 4. Unbound Service is independent of the component in which it is started.
 5. The life cycle methods of unbound service are:
 		
 		onCreate();
		onStartCommand();
		onDestroy();
	
 Bound Service :
 ===============
 
 1. Bound Service also called as BindSerice, An application component binds to it by calling bindService(). A bound service offers a client-server interface that allows components to interact with the service, send requests, get results, and even do so across processes with interprocess communication (IPC).
 2. Bounded Service gets starts by calling bindService().
 3. Bounded Service is unbind or destroyed by calling unbindService().
 4. Bound Service dependents on the component in which it is started.
 5. There are total 3 ways to bind a service with application components
    Using IBinder class
    Using Messanger class
    Using AIDL
 6. The Lifecycle methods of Bound service are: 
 		
		onCreate();
		onBind();
		onUnbind();
		onDestroy();
 
 7. Service Acts as a server, component which is bound acts as a client

Intent Service :
================
 
 1. Intent Service is used to perform one time task i.e when the task completes the service destroys itself.
 2. Intent Service gets  starts by calling startService().
 3. IntentService Implicitly calls stopself() to destroy
 4. Intent Service is independent of the component in which it is started.

	
	Service CallBack methods and Description:

		onCreate() This method is invoked when an Service is first created using onStartCommand() or onBind() This is required to perform one time setup.
	
		onStartCommand() This method is invoked when an activity starts the service by calling startService(). if we implement this method it's our responsibality to stop the service by calling stopself() or stopservice().
	
		onBind() This method is invoked when another component want to bind with the service by calling bindService(), If you implement this method we must provide an interface that clients used to communicate with this service by returning IBinder object , we must always implement this method , but if you dont want to allow binding simply return null.
	
		onUBind() This method is invoked when all clients have disconnected from a particular interface published by the service.
		onDestroy() The system is invoked when the service is no longer used and is being destroyed. Your service should implement this to clean up any resources such as threads, registered listeners, receivers, etc.


 NOTE: 
	onStartCommand() method has integer return type value which can be any of the following: 
	START_STICKY tells the OS to recreate the service after it has enough memory and call onStartCommand() again with a null intent.
    START_NOT_STICKY tells the OS to not bother recreating the service again.
	START_REDELIVER_INTENT that tells the OS to recreate the service AND redelivery the same intent to onStartCommand(). 

 
 Broadcast Receivers :
 =====================
 
 1. An android app can send or receives the broadcast message from the another app or from the android system itself.
 2. Broadcast message can be sent or received when an particular event occurs
 3. Example's like battery low,Incoming call, Message Received, Boot finished, Airplane mode on off, Internet connectivity available not available, Device starts charging etc...
 4. A broadcast can be received in 2 ways.
 
		1. manifest declared receiver / Static / Implicit
		2. context registered / Dynamic / Explicit
 5. Create a class that extends BroadcastReceiver and overide onreceive() method
 6. If we register a receiver in oncreate method we should unregister it in onDestroy() method (Keeps the broadcast for the lifecycle of an activity)
 7. If we register a receiver in onResume method we should unregister it in onPause() method
 8. If we register a receiver in onStart() method we should unregister it in onstop() method 
 9. To keep the broadcast reciver active as long as the  whole application is running we have to register in the oncreate() method of the application class

 Manifest Declared Receivers : If we declare a broadcast receiver in manifest it will trigger even if our application is not in foreground .
 
    <receiver android:name=".InternetBroadcast">
            <intent-filter>
                <action android:name="android.intent.action.AIRPLANE_MODE" />
            </intent-filter>
        </receiver>

Context Registered Receivers : If we declare a broadcast receiver in code it will trigger only if the appliction is in foreground.

        InternetBroadcast internetBroadcast = new InternetBroadcast();
        IntentFilter intentFilter = new IntentFilter();
        registerReceiver(internetBroadcast, intentFilter);

 Custom BroadcastReceiver
 
	<receiver android:name=".InternetBroadcast">
            <intent-filter>
                <action android:name="my.custom.broadcast" />
				<category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
	</receiver>
	
	In Activity class
	Intent intent = new Intent();
	intent.setAction(my.custom.broadcast);
	intent.addCategory(Intent.CATEGORY_DEFAULT);
	sendBroadacast(intent);
	
	1. To secure Broadcast use Exported = false so other application could'nt able to trigger your broadcast
	2. Another way to secure Broadcast receiver is LocalBroadcastManager
	3. Note: Above Nougat version If we declare a broadcast receiver in manifest it will not trigger even if our application is in foreground . we need to register dynamically in code
	
	
  Intents
 ========
 Intents is used to pass message or information between Android components, except Content Provider.
 
 1. Implicit Intent : Implicit intent is used to invoke system components.
	
        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.setData(Uri.parse("https://www.google.com"));
        startActivity(intent);

 2. Explicit Intent : Used to Invoke Activity class

        Intent intent = new Intent(FirstActivity.this, SecondActivity.class);
        intent.putExtra("KEY", "Value");
        startActivity(intent);

 3. Pending Intent	: Intent which you want to trigger at future point of time when your application is not alive
A PendingIntent is generally used in cases were an AlarmManager needs to be executed or for Notifications. A PendingIntent provides a mean for applications to work, even after their process exits.
	
		Intent notifyIntent = new Intent(this, ResultActivity.class);
        PendingIntent notifyPendingIntent = PendingIntent.getActivity(this, 0, notifyIntent, PendingIntent.FLAG_UPDATE_CURRENT);
        NotificationCompat.Builder builder = new NotificationCompat.Builder(this, CHANNEL_ID);
        builder.setContentIntent(notifyPendingIntent);
        NotificationManagerCompat notificationManager = NotificationManagerCompat.from(this);
        notificationManager.notify(NOTIFICATION_ID, builder.build());
 
 4. Sticky Intent	: Sticks with Android system, for future broadcast listener. 
	For example if BATTERY_LOW event occurs then that Intent will stick with Android system so that any future requests for BATTERY_LOW, will return the Intent.

		IntentFilter intentfilter = new IntentFilter(Intent.ACTION_BATTERY_CHANGED);
		Intent batteryStatus = context.registerReceiver(null, intentfilter);
 
 Content Providers
 ===================
 1. Content providers is nothing but it provide the data/content from one Android application to another android application or to Android system itself.
 2. It acts has a central repository to supply data from one application to another application on request.
 3. It stores data in various formats such as files,database or over Internet.
 4. Ex : Contacts app sharing contact's details with whatsapp based on request.
 	
	Steps to create Content Provider
	================================
	
    First of all you need to create a class that extends ContentProvider.

    Second, you need to define your content provider URI address which will be used to access the content.
	content://authority/path/optional_id
	content://contacts_info/users/5

    Next you will need to create your own database to keep the content. Usually, overide contentProviderMethods
	onCreate() , query() , insert() , update() , delete() , getType()

    Next you will have to implement ContentProvider queries to perform different database specific operations.

    Finally register your Content Provider in your activity file using <provider> tag.

 
	
 Context
 =======
 
 1. A Context is a handle to the system; it provides services like resolving resources, obtaining access to databases and preferences, and so on.
 2. An Android app has activities. Context is like a handle to the environment your application is currently running in.


 Application Context: 
 =======================

 1. This context is tied to the lifecycle of an application.
 2. The application context(getApplicationContext()) can be used where you need a context whose lifecycle is separate from the current context or when you are passing a context beyond the scope of an activity.
 3. Example Use: If you have to create a singleton object for your application and that object needs a context, always pass the application context.


 Activity Context
 ================

 1. This context is available in an activity.
 2. This context is tied to the lifecycle of an activity.
 3. The activity context should be used when you are passing the context in the scope of an activity or you need the context whose lifecycle is attached to the current context.


 MVVM in Android
 ===============
 
 There are three ways to implement MVVM in Android:
    Data Binding
    RXJava
	Live Data

 Android Architecture Components
 ===============================
 
 1. Android Architecture Components are part of Android JetPack 
 2. Android Architecture components contains a set of Libraries which helps to deal with the boilerplate code, managing Activity Lifecycle, configuration changes, memory Link
 3. Android Architecture components are
 	
	1. Data Binding  : DataBinding helps in binding the UI elements present in our layout to data sources of our app.
	2. Lifecycles	 : It manages the Activity and Fragment Lifecycle , It helps to avoid memory leaks and also take care of Configuration changes.
	3. ViewModel	 : viewModel stores UI-related data that isn't destroyed on app rotations.
	4. LiveData	 	 : LiveData takes in an observer and notifies it about data changes only when it is in STARTED or RESUMED state.
	5. Navigation	 : Navigation handles in-app navigations
	6. Paging		 : Paging helps to load the information on demand from datasource
	7. Room		 	 : Room is a wrapper of sqlite , it helps to avoid boilerplate code and easily convert Sqlite table data to java objects.
	8. WorkManager   : WorkManger manages every background jobs in Android with the circumstances we choose.

 NOTE::
 How is it possible to notify some class without having a reference of it?

 It can be done in three different ways:

    Using Two Way Data Binding
    Using Live Data
    Using RxJava
	
 The Advantage of using LiveData is
 
	1. Ensures your UI matches your data state.
	2. No memory leaks.
	3. No crashes due to stopped activities.
	4. No more manual lifecycle handling.
	5. Sharing resources.


 Data Binding
 =======================
 
 1. DataBinding layout is different than the normal layout
 2. It starts with the root Tag 
	
	<layout>
    <data>
        <variable
            name="employee"	//Example
            type="com.aravind.databinding.Employee" /> //Example
    </data>

    <LinearLayout
		...
    />
</layout>
 
 3. For each layout file ,it generates the corrosponding .class file which holds all the bindings from the layout properties
    for example the layout filename is activity_main.xml so the corresponding generated class is ActivityMainBinding.



 Two Way Data Binding
 =======================
 
 1. Two-way Data Binding is a technique of binding your objects to your XML layouts such that the Object and the layout can both send data to each other.
 2. In our case, the ViewModel can send data to the layout and also observe changes.
 3. For this, we need a BindingAdapter and custom attribute defined in the XML.


 @Bindable
 The Bindable annotation should be applied to any getter accessor method of an Observable class. Bindable will generate a field in the BR class to identify the field that has changed. 

View Model
==========
 1. ViewModel acts as a communication layer between Model and View
 2. ViewModel is a class reponsible for preparing, managing and providing data to the UI (Activity or Fragment).
 3. when the Activity is created link it to the ViewModel and the viewmodel survives until and unless the Activity which is created is completly destroyed
 4. ViewModel is not same as OnsavedInstanceState() , OnsavedInstanceState() is used to retain small amount of data but viewmodel is used for largeAmount of data like ProductsList etc.
 3. The ViewModel can retain it state across configuration changes.
 4. The viewModel stays in memory unti the lifecycle its scope goes away permanently (In case of activity, once it finisshes , In case of fragment once it is detached).
 5. If the viewModel needs a ApplicationContext than it can extend AndroidViewModel classs and have a constructor that receives application in the constructor.
 
 'Two steps to follow for ViewModel.'

 Step 1
	create a class that extends ViewModel or AndroidViewModel and write ur logics inside it.
	
	Ex : public class MainActivityViewModel extends ViewModel {
    // Logics
}

 Step 2 In Activity class 
	
	Example :
	MainActivityViewModel mainActivityViewModel = ViewModelProviders.of(MainActivity.this).get(MainActivityViewModel.class);
        textView.setText(mainActivityViewModel.getNumber());


LiveData
==========
 1. LiveData helps to communication between the ViewModel and View easy.
 1. LiveData basically a data holder that contains primitive/collection types.
 2. LiveData is based on the Observer Pattern, and it is LifeCycle Aware.
 3. LiveData notifies the observer using setValue() and postValue().
 4. setValue() runs on the main thread.	postValue() runs on the background thread.
 5. LiveData is immutable.
 
 
Mutable Live Data
=================

 1. MutableLiveData is a class that extends the LiveData type class.
 2. It provides the setValue(), postValue() methods publicly, something that LiveData class doesn’t provide.
 3. MutableLiveData is LiveData which is mutable & thread-safe.
 
 LifeCycle Aware Components
==========================

 1. LifeCycleOwner provides Lifecycle status to Lifecycle Aware components.
 2. LifeCycleObserver register the LifeCycle status to respond and perform action.
 3. LifeCycle owner uses following enumaration to track Lifecycle status
		Event
		State
 
 Imp Links
 =========
 
 https://www.quora.com/What-is-difference-between-listview-and-recyclerview-in-Android
 https://stackoverflow.com/questions/26570325/what-are-recyclerview-advantages-compared-to-listview
 
 