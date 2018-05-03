---
layout: post
title: YogaLayout Vs Litho Vs FlexboxLayout
date: 2018-04-19 10:00:00 +0300
img: flexbox.jpg # Add image post (optional)
tags: [yoga, YogaLayout, Litho, FlexboxLayout, Android]
---

## 比较
三个框架均为移动端FlexBox规范支持框架
1.公司
YogaLayout和Litho均为facebook出品，FlexboxLayout为google生产。

2.底层
facebook的东西都是基于yoga库进行的支持，该库为c实现，目前跨端支持CommoponentKit, Litho,
ReactNative，底层算法统一；FlexboxLayout是google出厂，底层为java实现，没有其他应用场景

3.API
YogaLayout目前只有三个类实现YogaLayout, VirtualYogaLayout和YogaLayoutFactory，接口较少，
对动态Flexbox属性不支持，仅支持xml中进行属性配置；

Litho支持丰富的基础组件，动态属性支持也很完整，
可能原因就是facebook对移动端flexbox的规划就是C,L,R三驾马车，所以YogaLayout只是一个测试性的东西；

FlexboxLayout虽然可以xml配置和动态修改属性，但支持的flexbox属性少得可怜，github项目最近的更
新是一个月以前，更新了一个demo，目前还不适合项目使用。

4.例子
YogaLayout
```
<com.facebook.yoga.android.YogaLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/ygcontainer"
            yoga:flex_direction="column"
            yoga:align_items="flex_start">
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="【直播中国地球卫士青年奖发布会：王俊凯、徐峥、惠若琪、董力将出席】"
                android:textColor="@android:color/black"
                android:textSize="14dp"
                yoga:flex="0"
                yoga:margin_left="10dp"
                />
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:src="@drawable/picture"
                yoga:flex="0"
                yoga:margin_left="10dp"
                />
        </com.facebook.yoga.android.YogaLayout>
```
Litho
```@LayoutSpec
public class PlaygroundComponentSpec {
	@OnCreateLayout
	static Component onCreateLayout(ComponentContext c) {
		return Row.create(c)
			.child(
				Row.create(c)
					.widthDip(100)
					.heightDip(100))
			.child(
				Row.create(c)
					.widthDip(100)
					.heightDip(100)
					.marginDip(YogaEdge.LEFT, 10))
			.child(
				Row.create(c)
					.widthDip(100)
					.heightDip(100))
			.child(
				Row.create(c)
					.widthDip(100)
					.heightDip(100)
					.marginDip(YogaEdge.LEFT, 20))
			.widthDip(500)
			.heightDip(500)
			.build();
	}
}
```

FlexboxLayout
```
<com.google.android.flexbox.FlexboxLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:flexWrap="wrap"
    app:alignItems="stretch"
    app:alignContent="stretch" >
    <TextView
        android:id="@+id/textview1"
        android:layout_width="120dp"
        android:layout_height="80dp"
        app:layout_flexBasisPercent="50%"
        />
    <TextView
        android:id="@+id/textview2"
        android:layout_width="80dp"
        android:layout_height="80dp"
        app:layout_alignSelf="center"
        />
</com.google.android.flexbox.FlexboxLayout>
```
或者
```
FlexboxLayout flexboxLayout = (FlexboxLayout) findViewById(R.id.flexbox_layout);
flexboxLayout.setFlexDirection(FlexDirection.ROW);
View view = flexboxLayout.getChildAt(0);
FlexboxLayout.LayoutParams lp = (FlexboxLayout.LayoutParams) view.getLayoutParams();
lp.order = -1;
lp.flexGrow = 2;
view.setLayoutParams(lp);
```