# 浏览器 History

允许在用户浏览历史中向前和向后跳转，同时从HTML5开始，提供了对history栈中内容的操作。
api：
- `history.back` 向后跳转，这和用户点击浏览器回退按钮的效果相同
- `history.forward` 向前跳转，如同用户点击了前进按钮
- `history.go` 载入到会话历史中的某一个特定页面，通过与当前页面相对位置来标志
添加和修改历史记录条目
HTML5 中引入了 `history.pushState` 和 `history.replaceState` 方法，他们分别可以添加和修改历史记录条目。它们常与 `window.onpopstate` 配合使用。

使用效果：
```js
// 当前浏览器地址 https://lxfriday/foo.html
var stateObj = { foo: 'bar' };
history.pushState(stateObj, 'page title', 'bar.html');
```
浏览器地址将会更改为：`https://lxfriday/bar.html`，但是浏览器并不会加载 `bar.html`，甚至不会检查 `bar.html` 是否存在（单页应用是在浏览器上面检测浏览器url变化，然后做出对应的渲染更改，并不会在url更改之后由浏览器自发的去获取网页）

`onpopstate` 事件将会在页面的url发生变动之后立即触发，其事件对象的属性含有 state（**其不会被pushState、replaceState触发**）。


- `history.pushState` 需要三个参数：一个状态对象，一个标题（目前被忽略），和一个可选的URL
    - 状态对象：其是一个JavaScript对象，通过pushState创建新的历史记录条目。无论什么时候用户导航到新的状态，popstate事件就会被触发，且该事件的state属性包含该历史记录条目状态对象的副本，状态对象可以是能被序列化的任何东西，ff将状态对象保存在用户的磁盘上，以便用户重启浏览器时使用，state在序列化表示后有 **640k** 大小限制，传入大于640k的对象，该方法会抛出异常
    - 标题：ff目前忽略
    - URL：该参数定义了新的历史URL记录.新URL必须与当前URL同源，否则会抛出异常，该参数可选，缺省为当前URL（document.URL）
- `history.replaceState` 是修改了当前的历史记录项而不是新建一个。其使用场景在于为了响应用户操作，你想要更新状态对象state或者当前历史记录的URL
