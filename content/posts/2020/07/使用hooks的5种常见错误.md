---
title: "使用hooks的5种常见错误"
date: 2020-07-14T13:51:30+08:00
draft: false
tags: ["javascript", "react", "hooks"]
categories: ["阅读笔记"]
---

> [Five common mistakes writing react components (with hooks) in 2020](https://www.lorenzweiss.de/common_mistakes_react_hooks/) 阅读笔记

## 1 在没有渲染需要的时候使用`useState`

这是一种`useState`被滥用的情况

```javascript
function ClickButton(props) {
  // count被定义，但是并没有在return中被渲染
  const [count, setCount] = useState(0);

  const onClickCount = () => {
    setCount((c) => c + 1);
  };

  const onClickRequest = () => {
    apiCall(count);
  };

  return (
    <div>
      <button onClick={onClickCount}>Counter</button>
      <button onClick={onClickRequest}>Submit</button>
    </div>
  );
}
```

### 问题 ⚡

每一次修改`state`都将强制渲染组件及其子组件，而上面例子中的`count`并没有参与到渲染中，因此每次修改`count`的值都将导致不必要的渲染，可能会影响性能或产生意料之外的副作用

### 解决方法 ✅

使用`useRef`代替`useState`来保存你想保存的值

```javascript
function ClickButton(props) {
  const count = useRef(0);

  const onClickCount = () => {
    count.current++;
  };

  const onClickRequest = () => {
    apiCall(count.current);
  };

  return (
    <div>
      <button onClick={onClickCount}>Counter</button>
      <button onClick={onClickRequest}>Submit</button>
    </div>
  );
}
```

## 2 使用`router.push`代替超链接

```javascript
function ClickButton(props) {
  const history = useHistory();

  const onClick = () => {
    history.push("/next-page");
  };

  return <button onClick={onClick}>Go to next page</button>;
}
```

### 问题 ⚡

这里主要的问题是针对屏幕阅读器而言，也就是对视障人士来说，使用 js 事件来代替超链接是一件非常不友好的事情，因为屏幕阅读器无法识别这个`button`点击后是一个超链接的效果，作者原文如下：

> Even though this would work just fine for most of the users, there is a huge problem when it comes to accessibility here. The button will not be marked as linking to another page at all, which makes it nearly impossible to be identified by screen readers. Also could you open that in a new tab or window? Most likely not.

### 解决方法 ✅

使用`<Link>`组件或者`<a>`标签来替代

```javascript
function ClickButton(props) {
  return (
    <Link to="/next-page">
      <span>Go to next page</span>
    </Link>
  );
}
```

## 3 通过`useEffect`来触发一些`action`动作

看到这个例子的时候，我感觉发现了新世界，真不敢相信有人会用这种方式来写这种逻辑

```javascript
function DataList({ onSuccess }) {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [data, setData] = useState(null);

  const fetchData = () => {
    setLoading(true);
    callApi()
      .then((res) => setData(res))
      .catch((err) => setError(err))
      .finally(() => setLoading(false));
  };

  useEffect(() => {
    fetchData();
  }, []);

  useEffect(() => {
    if (!loading && !error && data) {
      onSuccess();
    }
  }, [loading, error, data, onSuccess]);

  return <div>Data: {data}</div>;
}
```

### 问题 ⚡

这样做的话，我们就让`fetchData`和`onSuccess`这两个方法失去了直接联系，或者说我们不知道这两个方法的直接关系，无法保证`onSuccess`只会在`fetchData`成功后执行

### 解决方法 ✅

将`onSuccess`与`fetchData`直接关联起来

```javascript
function DataList({ onSuccess }) {
  const [loading, setLoading] = useState(false);
  const [err, setError] = useState(null);
  const [data, setData] = useState(null);

  const fetchData = () => {
    setLoading(true);
    callApi()
      .then((fetchedData) => {
        setData(fetchedData);
        onSuccess();
      })
      .catch((err) => setError(err))
      .finally(() => setLoading(false));
  };

  useEffect(() => {
    fetchData();
  }, []);

  return <div>{data}</div>;
}
```

## 4 单一职责组件

组件的拆分是很难的一件事情，主观性挺高的

```javascript
function Header(props) {
  return (
    <header>
      <HeaderInner menuItems={menuItems} />
    </header>
  );
}

function HeaderInner({ menuItems }) {
  return isMobile() ? (
    <BurgerButton menuItems={menuItems} />
  ) : (
    <Tabs tabData={menuItems} />
  );
}
```

#### 问题 ⚡

`HeaderInner`这个组件将产生两种情况，这不利于了解这个组件是做啥的，到底要输出啥，这也使得在其他地方测试或重用该组件变得更加困难

### 解决方法 ✅

将判断的条件提升到`Header`组件

```javascript
function Header(props) {
  return (
    <header>
      {isMobile() ? (
        <BurgerButton menuItems={menuItems} />
      ) : (
        <Tabs tabData={menuItems} />
      )}
    </header>
  );
}
```

## 5 单一职责的`useEffect`

在`class`写法中，我们有多个用于不同生命周期的钩子；而在`hooks`写法中，我们都集中到了`useEffect`这个`hooks`中

```javascript
function Example(props) {
  const location = useLocation();

  const fetchData = () => {
    /*  Calling the api */
  };

  const updateBreadcrumbs = () => {
    /* Updating the breadcrumbs*/
  };

  useEffect(() => {
    fetchData();
    updateBreadcrumbs();
  }, [location.pathname]);

  return (
    <div>
      <BreadCrumbs />
    </div>
  );
}
```

#### 问题 ⚡

当`location.pathname`变化时，将导致`fetchData`和`updateBreadcrumbs`都被调用，而实际我们只想调用`updateBreadcrumbs`

### 解决方法 ✅

使用多个`useEffect`将它们区分开

```javascript
function Example(props) {
  const location = useLocation();

  const updateBreadcrumbs = () => {
    /* Updating the breadcrumbs*/
  };

  useEffect(() => {
    updateBreadcrumbs();
  }, [location.pathname]);

  const fetchData = () => {
    /*  Calling the api */
  };

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <div>
      <BreadCrumbs />
    </div>
  );
}
```
