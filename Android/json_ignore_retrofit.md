
# Json Ignore for certain fields in our pojo class in retrofit (Annotation style: GSON)

Sometimes we might add some extra field in our pojo class for doing certain operations in app, also we might use the same pojo as post params retrofit post calls.

```For Example```
In the below example

```
public class UserDetailsParams {

    @SerializedName("name")
    private String name;

    @SerializedName("email")
    private String email;

    @SerializedName("mobile")
    private String mobile;
    
    @SerializedName("location")
    private String location;
    
    private boolean isLoggedIn;
}
```

In the above class name, email, mobile, and location which I need to sent to server, if I am using this in body params one more addition field will be added in body params as ```"isLoggedIn":false``` which I dont want to send. At the same time this class has to be Parcelable.

### To overcome this kind of problem, I found a solution of creating a custom annotation for json ignore certain feilds

```@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface JsonIgnore {
}
```
*Added a interface JsonIgnore, this can used an annotation as ```@JsonIgnore```

```
retrofit = new Retrofit.Builder()
                    .baseUrl(BASE_URL)
                    .addConverterFactory(GsonConverterFactory.create(new GsonBuilder()
                            .addSerializationExclusionStrategy(new ExclusionStrategy() {
                                @Override
                                public boolean shouldSkipField(FieldAttributes field) {
                                    return field.getAnnotation(JsonIgnore.class) != null;
                                }

                                @Override
                                public boolean shouldSkipClass(Class<?> clazz) {
                                    return false;
                                }
                            })
                            .setPrettyPrinting()
                            .setLenient()
                            .create()))
                    .client(client)
                    .build();
```
* While creating retrofit object we can override ExclusionStrategy for skipping.
So the pojo class with json ignore will be looks like below
```
public class UserDetailsParams {

    @SerializedName("name")
    private String name;

    @SerializedName("email")
    private String email;

    @SerializedName("mobile")
    private String mobile;
    
    @SerializedName("location")
    private String location;
    
    @JsonIgnore
    private boolean isLoggedIn;
}
```
                
