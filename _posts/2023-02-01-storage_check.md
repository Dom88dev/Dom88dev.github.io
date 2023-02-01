---
title: "안드로이드 저장소 여유 용량 체크"
layout: post
date: 2023-02-01 09:43 +0900
image: /assets/images/android_icon.png
headerImage: true
tag:
- android
- code_snippet
category: blog
author: dom
description: "안드로이드에서 내부및 외부 저장소 총 용량 및 현재 여유용량 체크 코드 스니펫"
---

## 내부 저장소 총 용량 사이즈 체크 코드

내부 저장소 총 용량 가져오기
{% highlight kotlin %}
fun getTotalInternalMemorySize() : Float {
    return StatFs(Environment.getDataDirectory().path).let {
        formatSizeFloatMb(it.blockSizeLong * it.blockCountLong)
    }
}
{% endhighlight %}

내부 저장소 여유 용량 사이즈 가져오기
{% highlight kotlin %}
fun getFreeInternalMemorySize() : Float {
    return StatFs(Environment.getDataDirectory().path).let {
        formatSizeFloatMb(it.blockSizeLong * it.availableBlocksLong)
    }
}
{% endhighlight %}

---

## 외부 저장소 용량 체크 코드

외부 저장소가 마운트되어있는지 체크
{% highlight kotlin %}
fun isExternalMemoryAvailable() : Boolean {
    return Environment.getExternalStorageState() == Environment.MEDIA_MOUNTED
}
{% endhighlight %}

외부 저장소 총용량 사이즈 가져오기
{% highlight kotlin %}
fun getTotalExternalMemorySize() : Float {
    return if (isExternalMemoryAvailable()) {
        StatFs(Environment.getExternalStorageDirectory().path).let {
            formatSizeFloatMb(it.blockSizeLong * it.blockCountLong)
        }
    } else 0f
}
{% endhighlight %}


{% highlight kotlin %}
fun getFreeExternalMemorySize() : Float {
    return if (isExternalMemoryAvailable())
        StatFs(Environment.getExternalStorageDirectory().path).let {
            formatSizeFloatMb(it.blockSizeLong * it.availableBlocksLong)
        }
    else 0f
}
{% endhighlight %}

---
> 상기 코드는 바이트사이즈를 가져오는 것 KB는 1024를, MB는 (1024*1024)를, GB는 (1024*1024*1024)를 나눠줘야 한다.
