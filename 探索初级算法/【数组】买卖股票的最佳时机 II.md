[原题地址](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

这个题我一开始想的是按照题目的三个示例分为三种方法来判断，先判断是否为递增或者递减的数组，那么就很容易输出需要的值，这里就只考虑了数组的元素为数字的情况。
```
  // prices为输入的数组
  let anotherPrice = Array.from(prices).sort()
  if (JSON.stringify(anotherPrice) === JSON.stringify(prices)) {
    return prices[prices.length - 1] - prices[0]
  }
  if (JSON.stringify(anotherPrice.reverse()) === JSON.stringify(prices)) {
    return 0
  }
```
然后再去判断第三个例子下的不同情况，首先拿第一个和第二个以及后面的去比较，然后再跳到第二个和第二个的第二个即第三个以及后面的比较等等。

```
  let amountMaxPrice = []
  for (let k = 0; k < prices.length; k++) {
    let flag = 0
    let maxPrice = []
    for (let i = k; i < prices.length - 1; i = flag + 1) {
      for (let j = i + 1; j < prices.length; j++) {
        let cj = 0
        if (prices[j] - prices[i] > cj) {
          cj = prices[j] - prices[i]
          maxPrice.push(cj)
          flag = j
          break
        }
      }
    }
    amountMaxPrice.push(maxPrice)
  }
  let amount = 0
  amountMaxPrice.forEach(item => {
    let everyAmount = 0
    item.forEach(value => {
      everyAmount += value
    })
    amount < everyAmount ? amount = everyAmount : amount
  })
```
可是上面这种方式提交之后就发现了一个问题，每次都是和相邻的去比较而没有跳过相邻的和下一个去比较。思考良久，发现实在是很麻烦，
于是看了一下评论，发现了[贪心算法](https://baike.baidu.com/item/%E8%B4%AA%E5%BF%83%E7%AE%97%E6%B3%95/5411800?fr=aladdin)，此算法意为
在局部寻找最优解，不断地缩小能选择的条件最终得出最优解或者接近最优解的答案，但是这个算法不能保证最后的解是最优的，不能求最大最小解的问题。在有一些
约束条件下可以使用此算法。

所以在这一题里面就直接当今天的价格比昨天高的时候，我就卖出赚取差价，然后再去判断。
```
let maxProfit = prices => {
  let total = 0
  let last = -1
  for (let price of prices) {
    if(last !== -1 && price > last) {
      total += price - last
    }
    last = price
  }
  return total
}
```
