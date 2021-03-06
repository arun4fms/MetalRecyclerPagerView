# MetalRecyclerPagerView

Heavy-heavy metal made RecyclerView look and feel like a ViewPager.

Additionally to the full charged power of RecyclerView you can setup margins for pager items to achieve the following effect:

![Alt text](/screenshot.png?raw=true "MetalRecyclerPagerView screenshot")

Let's rock!
-----
**1. Add MetalRecyclerPagerView into your layout**

```xml
<com.sandrlab.widgets.MetalRecyclerViewPager
    android:id="@+id/viewPager"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:itemMargin="32dp"/>
```

Here you can set **app:itemMargin** attribute to add item margin: it affects left and right margins only.

**2. Create pager item view layout**

Add **@+id/root_layout** id for your root item layout. For example:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/root_layout"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <!-- ... -->

    </android.support.v7.widget.CardView>

</FrameLayout>
```

**3. Create Adapter**

3.1 Create your adapter as usual for RecyclerView but inherit it from a new **MetalRecyclerViewPager.MetalAdapter**.

3.2 As for ViewHolder - also inherit it from **MetalRecyclerViewPager.MetalViewHolder** and call **super.onBindViewHolder(holder, position)**
within your *onBindViewHolder* implementation (don't forget to call it, item margins are not able to work properly without this thing).

```java
public class YourCoolAdapter extends MetalRecyclerViewPager.MetalAdapter<YourCoolAdapter.YourCoolViewHolder> {

    public YourCoolAdapter(@NonNull DisplayMetrics metrics, @NonNull List<String> yourDataSource) {
        super(metrics);
        
        // ...
    }

    @Override
    public void onBindViewHolder(FullMetalViewHolder holder, int position) {
        // don't forget to call supper.onBindViewHolder!
        super.onBindViewHolder(holder, position);

        // ...
    }

    static class YourCoolViewHolder extends MetalRecyclerViewPager.MetalViewHolder {

        public FullMetalViewHolder(View itemView) {
            super(itemView);

            // ...
        }
    }

    // ...
}
```

**4. Put it all together**

To instantiate your adapter you will need to pass **DisplayMetrics** into it.
No need to setup LayoutManager - it's already done within MetalRecyclerViewPager itself.

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        DisplayMetrics metrics = getDisplayMetrics();
        List<String> metalList = Arrays.asList("\\m/", "\\m/", "\\m/");
        MetalRecyclerViewPager.MetalAdapter fullMetalAdapter = new YourCoolAdapter(metrics, metalList);

        MetalRecyclerViewPager viewPager = (MetalRecyclerViewPager) findViewById(R.id.viewPager);
        viewPager.setAdapter(fullMetalAdapter);
    }

    private DisplayMetrics getDisplayMetrics() {
        Display display = getWindowManager().getDefaultDisplay();
        DisplayMetrics metrics = new DisplayMetrics();
        display.getMetrics(metrics);

        return metrics;
    }
}
```

Download
-----
Simply copy [MetalRecyclerViewPager.java](library/src/main/java/com/sandrlab/widgets/MetalRecyclerViewPager.java) along with [attrs.xml](library/src/main/res/values/attrs.xml) and [ids.xml](library/src/main/res/values/ids.xml) into your project.

Full sample project
-----
Find it [here](sample/).

Known issues
-----
As we have a recycler view here, it always takes focus: if you place MetalRecyclerViewPager into ScrollView Android will automatically scroll layout to MetalRecyclerViewPager.
To avoid this behavior place MetalRecyclerViewPager into [NestedScrollView](https://developer.android.com/reference/android/support/v4/widget/NestedScrollView.html) and add ```android:focusableInTouchMode="true"``` to upper container:

```xml
<android.support.v4.widget.NestedScrollView
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:focusableInTouchMode="true"
        android:orientation="vertical">
        
        <!-- ... -->
        
        <com.sandrlab.widgets.MetalRecyclerViewPager
            android:id="@+id/viewPager"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:itemMargin="32dp"/> 
            
        <!-- ... -->
        
    </LinearLayout>

</android.support.v4.widget.NestedScrollView>
```


License
-----
    Copyright (C) 2017. Alexander Bilchuk

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
