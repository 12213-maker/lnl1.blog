
---
title: js 实现链表
---
# js 实现链表 , 并判断链表是否循环 ,循环的长度,循环开始的点
## 一 . js实现链表

```javascript
//用js来实现一个链表

//构造节点
class Node {
    constructor(data) {
        this.data = data
        this.next = null
    }
}

//创建链表
class LinkedPropotype {
    constructor() {
        this.head = null
    }
    //构造节点
    create(data) {
        let node = new Node(data)
        if (this.head == null) {
            this.head = node
        } else {
            let current = this.head
            while (current.next) {
                current = current.next
            }
            current.next = node
        }
    }

    //删除节点
    deletenode(index) {
        let p = this.head
        let k = p
        let len = 0
        while (p) {
            k = p
            p = p.next
            len++
        }
        if (index < 0 || index > len) {
            console.log('删除超出范围');
            return;
        }

        let q = this.head
        let l = q
        //删除头节点
        if (index == 0) {
            this.head = this.head.next
            return q.data
        }
        //删除中间节点
        else if (index < len) {
            while (index) {
                l = q
                q = q.next
                index--
            }
            let deletenum = q.data
            l.next = q.next
            return deletenum
        }
        //删除尾节点
        else {
            k.next = null
            return p
        }
    }

    //查找节点
    searchnode(index){
        let p = this.head
        let k = p
        let len = 0
        while (p) {
            k = p
            p = p.next
            len++
        }
        if (index < 0 || index > len) {
            console.log('查找超出范围');
            return;
        }
        p = this.head
        while(index){
            index--
            p = p.next
        }
        return p
    }

    //修改节点
    editnode(index,data){
        let p = this.head
        let k = p
        let len = 0
        while (p) {
            k = p
            p = p.next
            len++
        }
        if (index < 0 || index > len) {
            console.log('修改超出范围');
            return;
        }
        p = this.head
        while(index){
            index--
            p = p.next
        }
        p.data = data
    }

}

let linknode = new LinkedPropotype()
linknode.create(1)
linknode.create(2)
linknode.create(3)
linknode.create(4)
linknode.create(5)
linknode.create(6)
linknode.create(7)
linknode.create(8)
//linknode.deletenode(2)
//linknode.editnode(4,888)
//linknode.searchnode(5)
```

## 二 . 判断一个链表是否循环 
 使用`` 快慢指针`` , 快指针每次都比慢指针多走一步 , 如果在快指针==没有走到null并且与慢指针相遇了== , 那么该链表就是循环链表
首先我们以上面的代码为基础构造一个循环链表 , 加上这三行代码
```javascript
var C = linknode.searchnode(4)
var G = linknode.searchnode(7)
G.next = C
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/450b3511a6c14e168fba34290e042381.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiN5Li66Zyc5YGc,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)

判断链表是否循环
```javascript
//使用  快慢指针  来判断有没有环
let isCircle = (link)=>{
    let p = link.head
    let q = link.head

    //快指针走完就没有必要再进行判断了
    while(q&&q.next){
        p = p.next
        q = q.next.next
        if(p === q){
            console.log('此链表循环');
            return;
        }
    }
    console.log('此链表不循环');
}
```

## 三 . 计算链表循环部分的长度
快慢指针第一次相遇 , 继续走, 当快慢指针第二次相遇的时候 					
 ``循环的长度  = ( 快慢指针速度差 ) * 前进次数 ``

```javascript

//变式一 : 求环的长度
let lengthOfLink = (link)=>{
    let p = link.head
    let q = link.head
    let flag= 0
    let count = 0
    while(q&&q.next){
        p = p.next
        q = q.next.next
        count++
        if(p===q){
            flag++
            if(flag==1)count = 0
            if(flag==2){
                return (2-1)*count
            }
        }
    }

}
```

## 四 . 计算入环点 
 当快慢指针第一次相遇的时候 , ``将其中一个指针置为head , 将两个指针都变为慢指针`` , 当两个指针第二次相遇的时候 , 前进的次数就是入环点距离head的距离

```javascript
//变式二 : 求环的切入点
let pointLink = (link)=>{
    let p = link.head
    let q = link.head
    let flag=0 
    let count = 0
    while(q&&q.next){
        p = p.next
        if(flag==0)
        q = q.next.next
        else 
        {
            q = q.next
            count++
        }
        if(p===q&&flag==0){
            q = link.head
            flag=1
        }
        if(p===q&&flag==1){
            return count
        }
    }

}
```



### 完整代码
```javascript


//用js来实现一个链表

//构造节点
class Node {
    constructor(data) {
        this.data = data
        this.next = null
    }
}

//创建链表
class LinkedPropotype {
    constructor() {
        this.head = null
    }
    //构造节点
    create(data) {
        let node = new Node(data)
        if (this.head == null) {
            this.head = node
        } else {
            let current = this.head
            while (current.next) {
                current = current.next
            }
            current.next = node
        }
    }

    //删除节点
    deletenode(index) {
        let p = this.head
        let k = p
        let len = 0
        while (p) {
            k = p
            p = p.next
            len++
        }
        if (index < 0 || index > len) {
            console.log('删除超出范围');
            return;
        }

        let q = this.head
        let l = q
        //删除头节点
        if (index == 0) {
            this.head = this.head.next
            return q.data
        }
        //删除中间节点
        else if (index < len) {
            while (index) {
                l = q
                q = q.next
                index--
            }
            let deletenum = q.data
            l.next = q.next
            return deletenum
        }
        //删除尾节点
        else {
            k.next = null
            return p
        }
    }

    //查找节点
    searchnode(index){
        let p = this.head
        let k = p
        let len = 0
        while (p) {
            k = p
            p = p.next
            len++
        }
        if (index < 0 || index > len) {
            console.log('查找超出范围');
            return;
        }
        p = this.head
        while(index){
            index--
            p = p.next
        }
        return p
    }

    //修改节点
    editnode(index,data){
        let p = this.head
        let k = p
        let len = 0
        while (p) {
            k = p
            p = p.next
            len++
        }
        if (index < 0 || index > len) {
            console.log('修改超出范围');
            return;
        }
        p = this.head
        while(index){
            index--
            p = p.next
        }
        p.data = data
    }

}

let linknode = new LinkedPropotype()
linknode.create(1)
linknode.create(2)
linknode.create(3)
linknode.create(4)
linknode.create(5)
linknode.create(6)
linknode.create(7)
linknode.create(8)


var C = linknode.searchnode(4)
var G = linknode.searchnode(7)
G.next = C

//使用  快慢指针  来判断有没有环
let isCircle = (link)=>{
    let p = link.head
    let q = link.head

    //快指针走完就没有必要再进行判断了
    while(q&&q.next){
        p = p.next
        q = q.next.next
        if(p === q){
            console.log('此链表循环');
            return;
        }
    }
    console.log('此链表不循环');
}

//变式一 : 求环的长度
let lengthOfLink = (link)=>{
    let p = link.head
    let q = link.head
    let flag= 0
    let count = 0
    while(q&&q.next){
        p = p.next
        q = q.next.next
        count++
        if(p===q){
            flag++
            if(flag==1)count = 0
            if(flag==2){
                return (2-1)*count
            }
        }
    }

}

//变式二 : 求环的切入点
let pointLink = (link)=>{
    let p = link.head
    let q = link.head
    let flag=0 
    let count = 0
    while(q&&q.next){
        p = p.next
        if(flag==0)
        q = q.next.next
        else 
        {
            q = q.next
            count++
        }
        if(p===q&&flag==0){
            q = link.head
            flag=1
        }
        if(p===q&&flag==1){
            return count
        }
    }

}



isCircle(linknode)

console.log('环的长度是',lengthOfLink(linknode));
console.log('环的切入点是',pointLink(linknode));

```