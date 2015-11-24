package com.ls.ui;

import android.content.Context;
import android.database.DataSetObserver;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.support.v4.view.ViewPager;
import android.util.AttributeSet;
import android.view.View;

/**
 * Created by user on 15-11-23.
 */
public class InkPageIndicator extends View implements ViewPager.OnPageChangeListener {

    //default
    private static final int DEFAULT_DOT_SIZE = 8;                      // dp
    private static final int DEFAULT_GAP = 12;                          // dp
    private static final int DEFAULT_UNSELECTED_COLOUR = 0x80ffffff;    // 50% white
    private static final int DEFAULT_SELECTED_COLOUR = 0xffffffff;      // 100% white

    //viewpager
    private ViewPager mViewPager;
    private int mPageCount;
    private boolean mIsAttachToWindow;

    //attributes
    private int dotDiameter;
    private int gap;

    //drawing
    private final Paint unselectedPaint;
    private final Paint selectedPaint;

    //state
    private int currentPage;
    private float[] dotCenterX;
    private float dotCenterY;
    private float dotTopY;
    private float dotBottomY;
    private float selectedDotX;


    public InkPageIndicator(Context context) {
        this(context, null, 0);
    }

    public InkPageIndicator(Context context, AttributeSet attrs) {
        this(context, attrs, 0);

    }

    public InkPageIndicator(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        final int density = (int) context.getResources().getDisplayMetrics().density;

        dotDiameter = DEFAULT_DOT_SIZE * density;

        unselectedPaint = new Paint();
        unselectedPaint.setColor(DEFAULT_UNSELECTED_COLOUR);
        selectedPaint = new Paint();
        selectedPaint.setColor(DEFAULT_SELECTED_COLOUR);
    }

    @Override
    protected void onAttachedToWindow() {
        super.onAttachedToWindow();
        mIsAttachToWindow = true;
    }

    @Override
    protected void onDetachedFromWindow() {
        super.onDetachedFromWindow();
        mIsAttachToWindow = false;
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int defaultHeight = getDefaultHeight();
        int height;
        switch (MeasureSpec.getMode(heightMeasureSpec)) {
            case MeasureSpec.EXACTLY: //wrap
                height = MeasureSpec.getSize(heightMeasureSpec);
                break;
            case MeasureSpec.AT_MOST: //fill, match
                height = Math.min(defaultHeight, MeasureSpec.getSize(heightMeasureSpec));
                break;
            default:
                height = defaultHeight;
                break;
        }
        int defaultWidth = getDefaultWidth();
        int width;
        switch (MeasureSpec.getMode(widthMeasureSpec)) {
            case MeasureSpec.EXACTLY:
                width = MeasureSpec.getSize(widthMeasureSpec);
                break;
            case MeasureSpec.AT_MOST:
                width = Math.min(defaultWidth, MeasureSpec.getSize(widthMeasureSpec));
                break;
            default: // MeasureSpec.UNSPECIFIED
                width = defaultWidth;
                break;
        }
        setMeasuredDimension(width, height);
        calculateDotePosition(width, height);
    }

    private int getDefaultHeight() {
        return getPaddingTop() + getPaddingBottom() + dotDiameter;
    }

    private int getDefaultWidth() {
        return getPaddingLeft() + getPaddingRight() + getRequiredWidth();
    }

    private int getRequiredWidth() {
        return mPageCount * dotDiameter + (mPageCount - 1) * gap;
    }

    private void calculateDotePosition(int width, int height) {
        int left = getPaddingLeft();
        int right = width - getPaddingRight();
        int top = getPaddingTop();
        int bottom = height - getPaddingBottom();

        int startLeft = left + dotDiameter;
        dotCenterX = new float[mPageCount];
        for (int i = 0; i < mPageCount; i++) {
            dotCenterX[i] = startLeft + (dotDiameter + gap) * i;
        }

        dotTopY = top;
        dotCenterY = top + dotDiameter / 2;
        dotBottomY = top + dotDiameter;

        setCurrentImmediate();
    }

    private void setCurrentImmediate() {
        if (mViewPager != null) {
            currentPage = mViewPager.getCurrentItem();
        } else {
            currentPage = 0;
        }
        if (dotCenterX != null) {
            selectedDotX = dotCenterX[currentPage];
        }
    }

    @Override
    protected void onDraw(Canvas canvas) {
        //super.onDraw(canvas);
        if (mViewPager == null || mPageCount == 0) {
            return;
        }
        drawSelected(canvas);
    }

    private void drawUnselected(Canvas canvas) {

    }

    private void drawSelected(Canvas canvas) {
        canvas.drawCircle(selectedDotX, dotCenterY, dotDiameter / 2, selectedPaint);
    }

    public void setupViewPager(ViewPager viewPager) {
        mViewPager = viewPager;
        mViewPager.addOnPageChangeListener(this);
        setPageCount(mViewPager.getAdapter().getCount());
        mViewPager.getAdapter().registerDataSetObserver(new DataSetObserver() {
            @Override
            public void onChanged() {
                super.onChanged();
                setPageCount(mViewPager.getAdapter().getCount());
            }
        });
    }

    private void setPageCount(int count) {
        mPageCount = count;
        //绘制dote
    }

    @Override
    public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

    }

    @Override
    public void onPageSelected(int position) {

    }

    @Override
    public void onPageScrollStateChanged(int state) {

    }
}
