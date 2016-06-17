SwipeBackLayout
===

An Android library that help you to build app with swipe back gesture.


![](https://github.com/Issacw0ng/SwipeBackLayout/blob/master/art/screenshot.png?raw=true)


Demo Apk
===
[GooglePlay](https://play.google.com/store/apps/details?id=me.imid.swipebacklayout.demo)

Download
===
[aar](https://github.com/babylikebird/SwipeBackLayout/blob/master/art/swipebacklayout.aar)

Usage
===
1. Add SwipeBackLayout as a dependency to your existing project.
2. To enable SwipeBackLayout, you can simply make your `Activity` extend `SwipeBackActivity`:
	* In `onCreate` method, `setContentView()` should be called as usual.
	* You will have access to the `getSwipeBackLayout()` method so you can customize the `SwipeBackLayout`. 
3. Make window translucent by adding `<item name="android:windowIsTranslucent">true</item>` to your theme.

修改内容
===
1.在使用systembarhint的时候，不生肖了。
  解决方法：在SwipeBackLayout的attachToActivity中修改如下
  ```
  //        ViewGroup decor = (ViewGroup) activity.getWindow().getDecorView();
        ViewGroup decor = (ViewGroup) activity.getWindow().getDecorView().findViewById(Window.ID_ANDROID_CONTENT);
  ```      
2.在布局文件设置的背景怎么没效果了？
  解决办法：在SwipeBackLayout的attachToActivity中有如下：
  ```
 TypedArray a = activity.getTheme().obtainStyledAttributes(new int[]{android.R.attr.windowBackground
});
 int background = a.getResourceId(0, 0);
a.recycle();
decorChild.setBackgroundResource(background);
```
把我们的背景给替换了，把这段注释掉。注意（注释掉之后，因为windowIsTranslucent设置为tue,那么就会透明了，需要在布局文件中设置背景）

3.滑动的时候怎么显示桌面？
  原因：在4.4系统以下最底层的的Activity的windowIsTranslucent设置为tue，会有此问题，如A->B，如果A设置windowIsTranslucent为true，那么就会出现此问题。5.0以上没问题。
  
Simple Example  
===
```
public class DemoActivity extends SwipeBackActivity implements View.OnClickListener {
    private int[] mBgColors;

    private static int mBgIndex = 0;

    private String mKeyTrackingMode;

    private RadioGroup mTrackingModeGroup;

    private SwipeBackLayout mSwipeBackLayout;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_demo);
        changeActionBarColor();
        findViews();
        mKeyTrackingMode = getString(R.string.key_tracking_mode);
        mSwipeBackLayout = getSwipeBackLayout();

        mTrackingModeGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup group, int checkedId) {
                int edgeFlag;
                switch (checkedId) {
                    case R.id.mode_left:
                        edgeFlag = SwipeBackLayout.EDGE_LEFT;
                        break;
                    case R.id.mode_right:
                        edgeFlag = SwipeBackLayout.EDGE_RIGHT;
                        break;
                    case R.id.mode_bottom:
                        edgeFlag = SwipeBackLayout.EDGE_BOTTOM;
                        break;
                    default:
                        edgeFlag = SwipeBackLayout.EDGE_ALL;
                }
                mSwipeBackLayout.setEdgeTrackingEnabled(edgeFlag);
                saveTrackingMode(edgeFlag);
            }
        });
    }
...
```


Pull Requests
===
I will gladly accept pull requests for fixes and feature enhancements but please do them in the develop branch.

License
===

   Copyright 2013 Issac Wong

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
