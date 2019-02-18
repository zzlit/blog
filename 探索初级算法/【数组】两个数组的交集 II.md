[原题地址](https://leetcode-cn.com/explore/interview/card/top-interview-questions-easy/1/array/26/)

首先能想到的最简单的解法，如下：
```
let intersect = (nums1, nums2) => {
  let arr = []
  for (const item of nums1) {
    if (nums2.indexOf(item) !== -1) {
      arr.push(item)
      nums2.splice(nums2.indexOf(item), 1)
    }
  }
  return arr
}
```
然后日常优化，好像暂时还没找到好的方法。。
