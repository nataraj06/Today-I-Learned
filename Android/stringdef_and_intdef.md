
# IntDef and StringDef in Android

We often use ```View```'s ```visibility``` in our apps to show and hide them. We use ```void setVisibility(int visibility)``` method for that purpose. But have you ever thought that why this method always takes ```VISIBLE```, ```INVISIBLE``` and ```GONE``` rather than any ```int``` value like 0 or 1 etc.? Although method's parameter type is ```int```, then why it doesn't accept the direct numbers or any other ```int``` variable except those three.

"Magic constants" or type annotations are regular old Java constants decorated in a way that allows tools to recognize them as special values. Their syntax is not different from any other Java constant. Instead, an annotation with an easily readable name which lists all related constants has to be created.

This annotation can then decorate a return value or a method parameter, giving hints to tools you use about the values that are accepted or can be expected. Their main purpose is to enforce type safety for method parameters and/or method return values while writing the source code.

This is typically used to replace enums. Google discourages the use of enums on Android app development as stated on their developer guidelines: "Enums often require more than twice as much memory as static constants. You should strictly avoid using enums on Android."

In java, ```enum``` is known concept, and in many cases you can use it, but for android, ```enum``` is something you should avoid to use as it’s processing performance is not efficient, so in Android performance patterns it’s told to avoid ```enum```s and to use annotations like ```@IntDef``` and ```@StringDef```

For this purpose, android has ```@IntDef``` and ```@StringDef``` annotations. Here's a simplest example how previously described problem can be made easy with ```@IntDef```.

```java
class MyPicLibrary
{
      @Retention(RetentionPolicy.SOURCE)                    //declare retention policy source i.e complile time
      @IntDef({OPEN_CAMERA, OPEN_GALLERY})                  // @IntDef value allocation 

      //Declare interface which we gonna use as our custom annotation 
      public @interface ImageSource { }

      // Must be static final int values for IntDef
      public static final int OPEN_CAMERA = 0;
      public static final int OPEN_GALLERY = 1;

      private int source;

      //As setImageSource has argument type of @ImageSource, parameters passed can only be one of both
      // OPEN_CAMERA or OPEN_GALLERY
      public void setImageSource(@ImageSource int source){
          this.source = source;
      }

      @ImageSource
      public int getImageSource(){
          return OPEN_CAMERA || OPEN_GALLERY;   //Here you can't return simple integer value, It will generate compile time error
      }
      
      public static void pickPhoto(@ImageSource int source)
      {
          if (source == OPEN_CAMERA)
          {
              // Camera intent goes here
          }
          else if (source == OPEN_GALLERY)
          {
              // Gallery intent goes here
          }
      }
}
```
******
The important topic here to understand is why we are not using ```enum```s? Although you can use it for similar purposes but they are not as memory efficient as ```static final int``` constants. Here's a nice video of performance optimization discussing the similar topic by Google below.

[Video Link for reference](https://www.youtube.com/watch?v=Hzs6OBcvNQE)
