## Android Interview ques and ans:

__Q.1 What is the diff b/w View Binding and Data Binding?__
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

__Q.2 What is the diff in  MutableLiveData, LiveData and MediatorLiveData?.__
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

__Q.3 How to create instance of ViewModel class in Fragment?__
Ans:

__In Fragment Use:__
viewModelRoutesFragment = new ViewModelProvider(requireActivity()).get(ViewModelRoutesFragment.class);

__instead of__
viewModelRoutesFragment = new ViewModelProvider(this).get(ViewModelRoutesFragment.class);

__Q.4 Difference Between MVP and MVVM Design Patter?.__
Ans:
![Capture1](https://user-images.githubusercontent.com/41982681/202150560-02db29a9-b8a0-4eb5-827b-f045b3a5e5b0.PNG)
![Capture3](https://user-images.githubusercontent.com/41982681/202150610-ab449b9f-4069-4e79-be9b-cf23e9465500.PNG)


         
   
       
       
