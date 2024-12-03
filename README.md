## Android Interview ques and ans:

__Q.1 What is the diff b/w View Binding and Data Binding?__<br>
Ans:
* __DataBinding__

apply plugin `kotlin-kapt` // if youre using kotlin
```
android {
    dataBinding {
        enabled = true
    }
```
* __ViewBinding__
```
android {
    viewBinding {
       enabled = true 
       }
   }
```  
Different between them is how to initialize that,
```
//DataBinding
binding = DataBindingUtil.setContentView(this, R.layout.activity_main) as ActivityMainBinding
//ViewBinding
binding = ActivityMainBinding.inflate(layoutInflater)
```

__Q.2 What is the diff in  MutableLiveData, LiveData and MediatorLiveData?.__<br>
Ans:
```
java.lang.Object
  ↳android.arch.lifecycle.LiveData<T>
     ↳android.arch.lifecycle.MutableLiveData<T>
         ↳android.arch.lifecycle.MediatorLiveData<T>
```
Now it is clear that MediatorLiveData is a subclass of MutableLiveData therefore MediatorLiveData can access each and every property of MutableLiveData as well as LiveData.

Question no. 1 is answered partially and rest of the answer will be discussed at the end of Question no. 2's answer.

After researching on some sample projects as well as android developer's official site I found that MutableLiveData should be used only for notifying your UI when observing any data.

For example, you want to display two SeekBars on two different fragments(Fragment1 and Fragment2) and you also want them to be synced when operating from Fragment1.

Another scenario is that we have 2 instances of LiveData, let's name them liveData1 and liveData2, and we want to merge their emissions in one object: liveDataMerger (which is a MediatorLiveData object). Then, liveData1 and liveData2 will become sources for the liveDataMerger and every time onChanged callback is called for either of them, we set a new value in liveDataMerger.

LiveData liveData1 = ...;
LiveData liveData2 = ...;

MediatorLiveData liveDataMerger = new MediatorLiveData<>();
liveDataMerger.addSource(liveData1, value ->liveDataMerger.setValue(value));
liveDataMerger.addSource(liveData2, value -> liveDataMerger.setValue(value));
In this case you cannot use MutableLiveData but on the other hand if you want to compare data into the first example (where MutableLiveData has been used) then you cannot because you will be unable to use the addSource property (as per class hierarchy).

__Q.3 How to create instance of ViewModel class in Fragment?__<br>
Ans:
__In Fragment Use:__
viewModelRoutesFragment = new ViewModelProvider(requireActivity()).get(ViewModelRoutesFragment.class);

__instead of__
viewModelRoutesFragment = new ViewModelProvider(this).get(ViewModelRoutesFragment.class);

__Q.4 Difference Between MVP and MVVM Design Patter?.__<br>
Ans:
![Capture1](https://user-images.githubusercontent.com/41982681/202150560-02db29a9-b8a0-4eb5-827b-f045b3a5e5b0.PNG)
![Capture3](https://user-images.githubusercontent.com/41982681/202150610-ab449b9f-4069-4e79-be9b-cf23e9465500.PNG)

__Q.5 What is the life cycle method when fragment1 add over fragmet 2 in same screen?__

Ans: In this case whatever fragment added last into the fragment stack android start that fragment first

![frag](https://user-images.githubusercontent.com/41982681/210847623-4ac99722-ece6-4ec8-bb50-592903596a4e.PNG)
![fragnent life cycle](https://user-images.githubusercontent.com/41982681/210847642-42507549-d62e-4f0a-9017-508aacedff4d.PNG)

__Q.6 Send data from one fragment to another fragment using interface?__<br>
Ans:
* add frag1 and another with their tag or id so that we can get them when ever required
* In this apporach we create one interface in fragment 1.
* and same interface we implement in activity where thes fragment added
* get the another fragment instance in activity by findbyId or findByTag in frag1 override method
* set fragment 1 value to frag 2 method<br>
Follow Below for more<br>
https://github.com/darshna22/FragmentBehaviousApp/tree/master

__Q.7 Send data from one fragment to another fragment using viewmodel class?__<br>
Ans:
* In this approach create sharable viewmodel class and create mutable livedata variable to set value and livedata variable to get set value
* get this view model instanse in both share and recieve fragment
* set value to mutable live data variable from share fragment
* observe frag1 value in frag2 using sharable view model class.<br>
For more ref plz follow below link:<br>
https://github.com/darshna22/FragmentBehaviousApp/tree/master

__Q.8 Create view model class instance in fragment?__<br>
Ans:![model](https://user-images.githubusercontent.com/41982681/210870932-25b6d6d0-2f2c-438d-a88c-691a730d729c.PNG)

__Q.9 Observe view model class data in fragment?__<br>
Ans:![observe](https://user-images.githubusercontent.com/41982681/210871040-8aa17b41-8054-470c-8d46-8d3ee49c4e7f.PNG)

__Q.10 what happens when you create view model class object as normal class like below?__
__val shareDataViewModel=ShareDataViewModel()__<br>
Ans: In this case this instance will not work observe viewLifecycleOwner<br>
result observer will not work for view which is observing its value.

__Q.11 what happens when fragment dialog in transparent background comes over another screen/fagment.?__<br>
Ans: Nothing happens all lifecycle works as they works.<br>
<p float="left">
<img src="https://user-images.githubusercontent.com/41982681/210877549-717089bb-38af-4fad-b47b-7eacc866e760.PNG" width="300" height="400"/>
<img src="https://user-images.githubusercontent.com/41982681/210877576-3fa9716c-929a-412d-8fd1-0bfcaa3a472b.png" width="300" height="400"/>
</p><br>
For more ref plz follow below link:<br>
https://github.com/darshna22/FragmentBehaviousApp/tree/master

__Q.12 ViewModel lifecycle and internal working?__ <br>
Ans: <br>
__Lifecycle__ <br>
A ViewModel's lifecycle begins when its associated UI controller is created and ends when the UI controller is destroyed or removed from the foreground. <br>
__Lifecycle methods__ <br>
ViewModels have two main lifecycle methods:<br>
onCreate(): Called when the ViewModel is first created, this is a good place to initialize data or perform setup tasks. <br>
onCleared(): Called when the associated UI controller is destroyed or the ViewModel is no longer in use, this is where cleanup tasks should be performed. <br>
__Internal workings__ <br>
ViewModels use ViewModelStore and ViewModelStoreOwner to manage their lifecycle and data retention. <br>
__Data sharing__ <br>
Multiple Fragments can share a common ViewModel to communicate and share data. <br>
__Configuration changes__ <br>
ViewModels can survive configuration changes, such as screen rotations, and retain data. This means that the UI doesn't need to fetch data again when navigating between activities or following configuration changes. <br>
__ViewModelProvider__ <br>
The ViewModelProvider class manages the lifecycle of a ViewModel. It can fetch or create a ViewModel, and will return an existing instance if one already exists within the lifecycle scope. <br>
__UI controller__ <br>
Never store a UI controller or Context directly or indirectly in a ViewModel. This can lead to memory leaks. <br>
![Uploading Screenshot from 2024-12-02 18.45.21.png…]()







         
   
       
       
