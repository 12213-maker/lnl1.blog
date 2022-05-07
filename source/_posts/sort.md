---
title: 排序算法总结
---

## 零 . 冒泡排序 ( js ) 时间复杂度: O(N2)
循环n次 , 每次把最大的冒泡到最后面

```javascript
let num = [2,8,6,5,1,2,3,4,5,6]

let bubbleSort = (arr)=>{
    for(let i = arr.length-1;i>=0;i--){
        for(let j = 0;j<=i;j++){
            if(arr[j]>arr[j+1]){
                [arr[j],arr[j+1]] = [arr[j+1],arr[j]]
            }
        }
    }
}

bubbleSort(num)
console.log(num);
```
## 一 . 堆排序(js)

 先把数组调整为大根堆 , 再把最顶部的元素与最后一个元素交换 , 交换之后再次进行堆调整 , 直到调整到最小值在顶部 

```javascript
let num = [1, 6, 12, 4, 5, 7, 8, 8, 9, 10, 0]

//调整最大堆 arr数组 , i 是每次调整的父节点  , len 是目前堆的大小
let downAdjust = (arr, i, len) => {
    //用左孩子来判断终止条件,在里面判断有没有右孩子 如果用右孩子判断的话,不确定结束的时候还有没有左孩子
    let leftchild = 2 * i + 1
    while (leftchild < len) {

        //判断右孩子
        if (leftchild+1 < len && arr[leftchild + 1] > arr[leftchild])
            leftchild++
        //父节点大直接跳出
        if (arr[i] >= arr[leftchild])
        break
        //交换节点
        [arr[i], arr[leftchild]] = [arr[leftchild], arr[i]]
        //下标
        i = leftchild
        leftchild = 2 * i + 1
        

    }
}

//创造堆 , 进行sort
let heapSort = (arr)=>{
    //把这个堆构建成最大堆. i代表的是父节点 , 显然从length-1开始是没有子节点的 
    //从有子节点的父节点开始构造堆 , 也就是i = (arr.length-1-1)/2  >>是除以二的意思
    for(let i = (arr.length-2)>>1;i>=0;i--){
        downAdjust(arr,i,arr.length)
    }
    console.log(arr ,'最大堆');

    //循环删除替换arr中最大元素到尾部
    for(let i= arr.length-1;i>0;i--){
        [arr[0],arr[i]] = [arr[i],arr[0]]
        //替换之后调整堆
        downAdjust(arr,0,i)
    }

}

console.log(num , '无序数组');
heapSort(num)
console.log(num , '排序好的数组');

```

## 二 . 快排 ( js ) 时间复杂度:O(nlogn)
每次选定一个基值 , 比基值小的放在左边 , 比基值大的放右边 , 用递归来实现 , 也可以使用

```javascript
//当数据量很大的时候 , 递归快排会造成栈溢出 , 为了解决这个问题 , 我们使用js数组 来模拟栈 , 
//将待排序的[left,right]保存到数组中 , 循环取出进行快排 
// let num = [4, 7, 3, 5, 6, 2, 8, 1]
let num = [1,3,2,5,9,6,8,7]
//非递归实现快排
const quickSort = (num, left, right) => {
    let flag = left
    let mark = left
    while (left <= right) {
        if (num[left] < num[flag]) {
            //这个时候mark就要往右移动一位 , 因为找到了一个小于flag 的数
            mark++
            [num[left], num[mark]] = [num[mark], num[left]]
        }
        left++
    }
    //交换基准数 和 mark 的值
    [num[flag], num[mark]] = [num[mark], num[flag]]
    //最后要返回基准数
    return mark
}

//使用非递归的方式进行快排
const jisuan = (num,left,right)=>{
    let list = [[left,right]]
    while(list.length!=0){
        let now = list.pop()
        if(now[0]>=now[1])
        continue;//结束这次循环去到下一个list.pop
        let flag = quickSort(num,now[0],now[1])
        //flag-1 和 flag+1 避免了类似[1,3,2,5,9,6,8,7]这样flag一直卡在0的情况
        list.push([now[0],flag-1])
        list.push([flag+1,now[1]])
    }
}
jisuan(num,0,num.length-1)
console.log(num);
```
## 三 . 归并排序 O(nlogn)
分开排序再合并起来
```javascript
const merger = (leftArr,rightArr) => {
    let left_len = leftArr.length
    let right_len = rightArr.length
    let arr = []
    let i = 0
    let j = 0
    while(i<left_len && j<right_len){
        leftArr[i]<rightArr[j] ? arr.push(leftArr[i++]) : arr.push(rightArr[j++])
    }
    while(i<left_len){
        arr.push(leftArr[i++])
    }
    while(j<right_len){
        arr.push(rightArr[j++])
    }
    return arr
}
const mergeSort = (arr) => {
    let length = arr.length
    if (length <= 1) {
        return arr
    }
    let mid = Math.floor(length/2)
    let leftArr = mergeSort(arr.slice(0,mid))
    let rightArr =mergeSort(arr.slice(mid))
    return merger(leftArr ,rightArr );
}

let num = [2,5,3,1,5,6,2,1,10,9,5,2,]

console.log(mergeSort(num));
```

## 四 . 插入排序 ( java ) O(n2)
```java
package class01;
//插入排序: 保证第一层循环i前面的元素全部都是有序的排序 
public class sort_charu {
	
	
	public static void charusort(int[] arr) {
		if(arr.length<2||arr==null)return;
		
		for(int i = 1;i<arr.length;i++) {
			for(int j = i-1;j>=0&&arr[j]>arr[j+1];j--) {
				swap(arr, j+1, j);
			} 
			
		}
		//直接输出arr是输出arr的首地址
		for(int i : arr) {
			System.err.print(i);
		}
	}
	
	public static void swap(int[] arr,int i,int j) {
		arr[i] = arr[i]^arr[j];
		arr[j] = arr[i]^arr[j];
		arr[i] = arr[i]^arr[j];
	}
	
	public static void main(String[] args) {
//		int[] arr = new int[]{0,5,1,3,5,4,7,6,2};
		int[] arr = {1,2,3,4,5,6,2,3,4,5,6,7,8,8,8,8};
		charusort(arr);
	}
}
```

## 五 . 选择排序  O(n2)
每次都选择最小的或者最大的放在开头或者放在结尾
```javascript
//每次遍历都找到最小的元素 , 放到数组的最开始
let num = [2,8,6,5,1,2,3,4,5,6]

let SelectSort = (arr)=>{
    for(let i = 0;i<arr.length-1;i++){
        let min = i
        for(let j=i+1;j<arr.length-1;j++){
            min = arr[j]<arr[min]?j:min
        }
        [arr[i],arr[min]] = [arr[min],arr[i]]
    }


    console.log(arr);
}
SelectSort(num)
```

## 六 . 桶排序 ( js )

```javascript
//桶排序 , 桶的数量就是要排序数组的长度  ,  桶区间就是(max-min)/桶数量

let num = [0.5, 0.84, 2.18, 3.25, 4.5]

let sort = (num) => {
    let max = num[0]
    let min = num[0]
    num.forEach((item) => {
        max = item > max ? item : max
        min = item < min ? item : min
    })

    //初始化桶
    let n = num.length
    //桶区间就是(max-min)/桶数量
    let d = (max - min) / (n - 1)

    let arr = []
    for (let i = 0; i < n; i++) {
        arr[i] = []
    }

    //遍历原数组 , 将每个元素放入桶中
    num.forEach((item) => {
        //item要在每个桶里面都看一看
        //index代表桶

        arr.forEach((item1, index) => {
            let left = min + (index) * d
            let right = min + (index + 1) * d
            if (item >= left && item < right)
                arr[index].push(item)
        })
    })

    let arr2 = []

    //桶已经放好了 , 接下来就是对每个桶进行排序
    arr.forEach((item,index)=>{
        item.sort((a,b)=>a-b)
        arr2.push(...item)
    })

    return arr2

}

console.log(sort(num));
```