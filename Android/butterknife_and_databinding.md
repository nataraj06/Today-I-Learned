# Android Butterknife vs Data Binding

Lets compare ```Butterknife``` and ```Data Binding``` libraries, which one is more advanced as of now.
If you’re not familiar with those two, in few words: they make your layout building experience better. 
And when comparing them I’ll go right from setting up and cover every basic aspect so you’ll learn how to use both of them!

## Butterknife

Butterknife is a light weight library to inject views into Android components. 
It uses annotation processing.

The ```@BindView``` annotation allow to inject views and performs the cast to the correct type for you. 
The ```@@OnClick(R.id.yourid)``` annotation allows to add OnClickListener to a view. You can optional define the method parameter
of the view in case you want it injected.

Butterknife includes also ```findById``` methods which simplify code that still has to find views on a View, Activity, or Dialog. 
It uses generics to infer the return type and automatically performs the cast.

##Data Binding
The Data Binding Library is a support library that allows you to bind UI components in your layouts to data sources in your 
app using a declarative format rather than programmatically.

Layouts are often defined in activities with code that calls UI framework methods. 
For example, the code below calls ```findViewById()``` to find a ```TextView``` widget and bind it to the userName property of the
viewModel variable:
```
TextView textView = findViewById(R.id.sample_text);
textView.setText(viewModel.getUserName());
```

## Setup

### Butterknife
For Butterknife you need to add those dependencies to build.gradle

```
compile ‘com.jakewharton:butterknife:8.6.0’
annotationProcessor ‘com.jakewharton:butterknife-compiler:8.6.0’
```

### DataBinding

Here you’ll just add those inside android scope in build.gradle

```
android{
…
dataBinding {
enabled true
}
}
```
And then wrap all your layouts with layout tag so it’s the root tag everywhere

```
<layout>
<LinearLayout>…</LinearLayout>
</layout>
```


### Butterknife
With Butterknife you can just declare fields with this annotation

```
public class MyActivity{
@BindView(R.id.textView) TextView text;
…
}
````
And declare each view as field this way, no need for findViewById method. 
Now if you compare this to old school method — you saved yourself 50% of lines if you save views as fields. Twice less lines.

But still, if you have 20 views — you’ll have those 20 ugly fields on top of the class, 
that’s just views, how about variables etc?

### DataBinding
Now that’s the next level, here you need to do some changes though. Replace default setContentView with this

```
@Override
protected void onCreate(Bundle savedInstanceState) {
ActivityMainBinding binding = DataBindingUtil.setContentView(this, R.layout.activity_main);
…
}
```
As you see there’s some ActivityMainBinding class — this class is generated for your layouts,
it’s based on layout name converted to camel case adding Binding at the end.
It happens when you wrap your layout with layout tag. If you have layout called layout.xml — you’d have LayoutBinding 
class generated

Now to access any of your views you just use

``` binding.textView ```
If you realized the advantage of Data Binding already — it’s you have 2 lines of code max.
You just save that binding variable as field if you need and can assess any of you fields anywhere within this class

* Conclusion *
Let’s say we have 20 views in our layout and we want them to be accessible in any method within a class, 
so we need to have them as fields.

With findViewById you’d have 40 lines of code, with Butterknife — 20, with DataBinding — 2.
Two lines! Data Binding is obvious winner here..

 [Click for a demo app here](https://github.com/nataraj06/DataBinding)
