# ListViewInsideScrollView

Insetead of <ListView>...</ListView> use <com.example.priyankam.listviewinsidescrollview.NonScrollListView>...</com.example.priyankam.listviewinsidescrollview.NonScrollListView> in Xml file.
And in MainActivity:  NonScrollListView listOne;
listOne=(NonScrollListView)findViewById(R.id.list_one);



Instead of <ScrollView/> use <com.example.priyankam.listviewinsidescrollview.VerticalScrollview>...</com.example.priyankam.listviewinsidescrollview.VerticalScrollview> in ScrollView
And in NewActivity:   VerticalScrollview scrollView;
scrollView=(VerticalScrollview)findViewById(R.id.scroll_view);

NonScrollListView.java

package com.example.priyankam.listviewinsidescrollview;

import android.content.Context;
import android.util.AttributeSet;
import android.widget.ListView;

public class NonScrollListView extends ListView {
    boolean expanded = true;
    public NonScrollListView(Context context) {
        super(context);
    }
    public NonScrollListView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    public NonScrollListView(Context context, AttributeSet attrs, int defaultStyle) {
        super(context, attrs, defaultStyle);
    }
    public boolean isExpanded() {
        return expanded;
    }
    public void setExpanded(boolean expanded) {
        this.expanded = expanded;
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        // HACK! TAKE THAT ANDROID!
        if (isExpanded()) {
            // Calculate entire height by providing a very large height hint.
            // View.MEASURED_SIZE_MASK represents the largest height possible.
            int expandSpec = MeasureSpec.makeMeasureSpec(MEASURED_SIZE_MASK,
                    MeasureSpec.AT_MOST);
            super.onMeasure(widthMeasureSpec, expandSpec);

            android.view.ViewGroup.LayoutParams params = getLayoutParams();
            params.height = getMeasuredHeight();
        } else {
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        }
    }
}

VerticalScrollview.java

package com.example.priyankam.listviewinsidescrollview;

import android.content.Context;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.widget.ScrollView;

public class VerticalScrollview extends ScrollView {
    public VerticalScrollview(Context context) {
        super(context);
    }

    public VerticalScrollview(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public VerticalScrollview(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        final int action = ev.getAction();
        switch (action)
        {
            case MotionEvent.ACTION_DOWN:
                Log.i("VerticalScrollview", "onInterceptTouchEvent: DOWN super false" );
                super.onTouchEvent(ev);
                break;

            case MotionEvent.ACTION_MOVE:
                return false; // redirect MotionEvents to ourself

            case MotionEvent.ACTION_CANCEL:
                Log.i("VerticalScrollview", "onInterceptTouchEvent: CANCEL super false" );
                super.onTouchEvent(ev);
                break;

            case MotionEvent.ACTION_UP:
                Log.i("VerticalScrollview", "onInterceptTouchEvent: UP super false" );
                return false;

            default: Log.i("VerticalScrollview", "onInterceptTouchEvent: " + action ); break;
        }

        return false;
    }

    @Override
    public boolean onTouchEvent(MotionEvent ev) {
        super.onTouchEvent(ev);
        Log.i("VerticalScrollview", "onTouchEvent. action: " + ev.getAction() );
        return true;
    }

}

OutPut:

![](https://raw.githubusercontent.com/Priyanka-Mohanty/ListViewInsideScrollView/master/Screenshot_20170105-125345.png)
![](https://raw.githubusercontent.com/Priyanka-Mohanty/ListViewInsideScrollView/master/Screenshot_20170105-125349.png)
![](https://raw.githubusercontent.com/Priyanka-Mohanty/ListViewInsideScrollView/master/Screenshot_20170105-132243.png)
