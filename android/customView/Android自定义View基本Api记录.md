# Android自定义View基本类和api记录
## 基本工具类
- Paint(画笔)
- Canvas(画布)
- Path(描点)
## 画一条线

```Java
package org.jiangtao.customview.widget;

import android.annotation.TargetApi;
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.os.Build;
import android.util.AttributeSet;
import android.view.View;
import org.jiangtao.customview.R;

/**
 * Created by kevin on 16-5-16.
 * Draw a line
 */
public class CustomViewLine extends View {

  private Paint mPaint;

  public CustomViewLine(Context context) {
    super(context);
    init();
  }

  public CustomViewLine(Context context, AttributeSet attrs) {
    super(context, attrs);
    init();
  }

  public CustomViewLine(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    init();
  }

  /**
   * init
   */
  public void init() {
    mPaint = new Paint();
    mPaint.setAntiAlias(true);
    mPaint.setColor(getResources().getColor(R.color.colorAccent));
    mPaint.setStyle(Paint.Style.FILL);
    mPaint.setStrokeWidth(5);
  }

  @Override protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);
  }

  @Override protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    canvas.drawColor(getResources().getColor(R.color.colorPrimaryDark));
    canvas.drawLine(0, 100, getWidth(), 100, mPaint);
  }
}

```

①UNSPECIFIED：父View没有对子View施加任何约束。它可以是任何它想要的大小。
②EXACTLY：父View已经确定了子View的确切尺寸。子View将被限制在给定的界限内，而忽略其本身的大小。
③AT_MOST：子View的大小不能超过指定的大小

MeasureSpec.UNSPECIFIED The parent has not imposed any constraint on the child. It can be whatever size it wants. 这种情况比较少，一般用不到。标示父控件没有给子View任何显示- - - -对应的二进制表示: 00
MeasureSpec.EXACTLY The parent has determined an exact size for the child. The child is going to be given those bounds regardless of how big it wants to be. 理解成MATCH_PARENT或者在布局中指定了宽高值，如layout:width=’50dp’. - - - - 对应的二进制表示:01
MeasureSpec.AT_MOST The child can be as large as it wants up to the specified size.理解成WRAP_CONTENT,这是的值是父View可以允许的最大的值，只要不超过这个值都可以。- - - - 对应的二进制表示:10
