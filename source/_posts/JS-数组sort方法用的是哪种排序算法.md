---
title: JS-数组sort方法用的是哪种排序算法
date: 2019-04-10 15:14:55
tags: 
    - javaScript
categories: 
    - javaScript
cover: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1557223877171&di=732f7ed4d00db4158561afc049ab5894&imgtype=0&src=http%3A%2F%2Fimg.mukewang.com%2F59ae0e2a0001307706000338.jpg
---
> JS数组的排序方法大家肯定用的太多了，那sort用的是什么排序算法呢？这个问题的答案我寻找了很久，最终在Chrome V8引擎的源码中找到了。

说明一下，ECMAScript没有定义使用哪种排序算法，各个浏览器的实现方式会有不同。火狐中使用的是归并排序，下面是Chrome的sort排序算法的实现。

### sort方法源码

```
DEFINE_METHOD(
  GlobalArray.prototype,
  sort(comparefn) {
    CHECK_OBJECT_COERCIBLE(this, "Array.prototype.sort");

    if (!IS_UNDEFINED(comparefn) && !IS_CALLABLE(comparefn)) {
      throw %make_type_error(kBadSortComparisonFunction, comparefn);
    }

    var array = TO_OBJECT(this);
    var length = TO_LENGTH(array.length);
    return InnerArraySort(array, length, comparefn);
  }
);
```

这一步看出sort方法调用了InnerArraySort方法，参数是数组，数组长度，比较函数。再看看InnerArraySort方法是如何处理的。

### InnerArraySort方法源码

```
function InnerArraySort(array, length, comparefn) {
  // In-place QuickSort algorithm.
  // For short (length <= 10) arrays, insertion sort is used for efficiency.

  if (!IS_CALLABLE(comparefn)) {
    comparefn = function (x, y) {
      if (x === y) return 0;
      if (%_IsSmi(x) && %_IsSmi(y)) {
        return %SmiLexicographicCompare(x, y);
      }
      x = TO_STRING(x);
      y = TO_STRING(y);
      if (x == y) return 0;
      else return x < y ? -1 : 1;
    };
  }
  function InsertionSort(a, from, to) {
    ...
  };
 ...
  function QuickSort(a, from, to) {
    var third_index = 0;
    while (true) {
      // Insertion sort is faster for short arrays.
      if (to - from <= 10) {
        InsertionSort(a, from, to);
        return;
      }
      if (to - from > 1000) {
        third_index = GetThirdIndex(a, from, to);
      } else {
        third_index = from + ((to - from) >> 1);
      }
      // Find a pivot as the median of first, last and middle element.
      var v0 = a[from];
      var v1 = a[to - 1];
      var v2 = a[third_index];
      var c01 = comparefn(v0, v1);
      if (c01 > 0) {
        // v1 < v0, so swap them.
        var tmp = v0;
        v0 = v1;
        v1 = tmp;
      } // v0 <= v1.
      var c02 = comparefn(v0, v2);
      if (c02 >= 0) {
        // v2 <= v0 <= v1.
        var tmp = v0;
        v0 = v2;
        v2 = v1;
        v1 = tmp;
      } else {
        // v0 <= v1 && v0 < v2
        var c12 = comparefn(v1, v2);
        if (c12 > 0) {
          // v0 <= v2 < v1
          var tmp = v1;
          v1 = v2;
          v2 = tmp;
        }
      }
      // v0 <= v1 <= v2
      a[from] = v0;
      a[to - 1] = v2;
      var pivot = v1;
      var low_end = from + 1;   // Upper bound of elements lower than pivot.
      var high_start = to - 1;  // Lower bound of elements greater than pivot.
      a[third_index] = a[low_end];
      a[low_end] = pivot;

      // From low_end to i are elements equal to pivot.
      // From i to high_start are elements that haven't been compared yet.
      partition: for (var i = low_end + 1; i < high_start; i++) {
        var element = a[i];
        var order = comparefn(element, pivot);
        if (order < 0) {
          a[i] = a[low_end];
          a[low_end] = element;
          low_end++;
        } else if (order > 0) {
          do {
            high_start--;
            if (high_start == i) break partition;
            var top_elem = a[high_start];
            order = comparefn(top_elem, pivot);
          } while (order > 0);
          a[i] = a[high_start];
          a[high_start] = element;
          if (order < 0) {
            element = a[i];
            a[i] = a[low_end];
            a[low_end] = element;
            low_end++;
          }
        }
      }
      if (to - high_start < low_end - from) {
        QuickSort(a, high_start, to);
        to = low_end;
      } else {
        QuickSort(a, from, low_end);
        from = high_start;
      }
    }
  };

  ...

  QuickSort(array, 0, num_non_undefined);
 ...
  return array;
}
```

这一步最重要的是QuickSort，从代码和注释中可以看出sort使用的是**插入排序和快速排序**结合的排序算法。数组长度不超过10时，使用插入排序。长度超过10使用快速排序。在数组较短时插入排序更有效率。

问题终于找到了答案，心里又少了一个挂念的问题。