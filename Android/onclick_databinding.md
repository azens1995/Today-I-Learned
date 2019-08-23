# How can we use click listener using the DataBinding and interface in recyclerview?
Usually, without using  data binding, we can implement click listener in the RecyclerView Adapter by passing the interface in the constructor of the adapter, and implementing that interface in *onBindViewHolder()*.

Using databinding, it has really become easy to implement the click listener interface.
We can do it in the follwing manner.
1. Create an interface for the clik event.
```
public interface CategoryClickListener {
    void onCategoryClick(CategoryDTO categoryDTO);
}
```

2. Create a variable for the click listener in the xml file.
```
<variable
            name="categoryClick"
            type="com.example.azens1995.CategoryClickListener" />
```

3. Use `android:onClick="..."` action on the view which has to trigger the click event.
```
<TextView
    ...

    android:onClick="@{() -> categoryClick.onCategoryClick(category) }"
    ... >
```

4. Create a variable for the `Context` and,  constructor for the `RecyclerView Adapter` that takes context as the parameter.
```
private Context mContext;

public ExampleAdapter(Context context, List<CategoryDTO> categoryList){
    this.mContext = context;
    ...
}
```

5. In the `ViewHolder` inner class, create a method that takes the Model and interface as the arguments, and set the model as well as interface to the xml layout.
```
LayoutBinding layoutBinding;

private void bindData(CategoryDTO category, CategoryClickListener clickListener){
    layoutBinding.setCategory(category);
    layoutBinding.setCategoryClick(clickListener);        
}
```

6. In `onBindViewHolder` pass the data to the earlier created method, and context which is being casted to the interface.
```
 holder.bindData(categoryList.get(position), (CategoryClickListener)mContext);
```

7. In the activity, implement the interface and perform your onClick action in the overridden method.