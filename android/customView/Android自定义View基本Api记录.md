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
