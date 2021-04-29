# fetch 方法

来代替原生ajax。。

![mark](http://qiniu.wind-zhou.com/blog/210412/29eAlC4cBh.png?imageslim)





![mark](http://qiniu.wind-zhou.com/blog/210412/b7jeFejEmF.png?imageslim)



>其实fetch，使用的是promise

（1）使用promise对象

（2）默认不发送cookie

（3）只有在网络故障或请求被阻止时，才会被标记为reject



但我们要才能判断到底是有没有返回数据呢？很简单，使用状态码就可以。





![mark](http://qiniu.wind-zhou.com/blog/210412/EC0AglkFb9.png?imageslim)

处理数据一般就是存放在state中。

fetch里但请求为get方法时，可以只传一个url。

![mark](http://qiniu.wind-zhou.com/blog/210412/HLAD1l473h.png?imageslim)







![mark](http://qiniu.wind-zhou.com/blog/210413/AJA43fH68E.png?imageslim)





 状态错误304，是指本地已经有缓存数据。



```js
// Example POST method implementation:

postData('http://example.com/answer', {answer: 42})
  .then(data => console.log(data)) // JSON from `response.json()` call
  .catch(error => console.error(error))

function postData(url, data) {
  // Default options are marked with *
  return fetch(url, {
    body: JSON.stringify(data), // must match 'Content-Type' header
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, same-origin, *omit
    headers: {
      'user-agent': 'Mozilla/4.0 MDN Example',
      'content-type': 'application/json'
    },
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, cors, *same-origin
    redirect: 'follow', // manual, *follow, error
    referrer: 'no-referrer', // *client, no-referrer
  })
  .then(response => response.json()) // parses response to JSON
}
```





数据交互：

CMS

增加员工：后台添加成功后，会跳转到查询界面（新添加的便显示了出来）

查询：有个搜索，信息太多会分页。  懒加载 （1）**ajax分页缓存**：























