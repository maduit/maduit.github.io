---
title: reat-router
tag: reat
categories: 2022
---
随意使用您选择的打包器，例如 [Create React App](https://create-react-app.dev/) 

```
npx create-react-app router-tutorial
```
<!-- more -->
然后安装 React Router 依赖项：

```
cd router-tutorial
npm install react-router-dom@6 history@5
```

![1668929643220](/images/router/1.png)

然后编辑你的App.js，让它变得很无聊:

```typescript
export default function App() {
  return (
    <div>
      <h1>Bookkeeper!</h1>
    </div>
  );
}
```

最后，确认`index.js` or `main.jsx`（取决于你的打包工具）是可用的：

root渲染App

```tsx
import { render } from "react-dom";
import App from "./App";

const rootElement = document.getElementById("root");
render(<App />, rootElement);

```

![1668929879392](/images/router/2.png)

然后是删了一点东西

![1668930704957](/images/router/3.png)

启动您的React应用：

```
npm start
```

## 连接路由

首先，我们想把你的应用连接到路由: import ' BrowserRouter '，并用它包裹你的整个应用。(修改index.js)

```
import { render } from "react-dom";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

const rootElement = document.getElementById("root");
render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  rootElement
);
```

应用程序中没有任何变化，但现在我们已准备好开始处理路由。

## 添加一些链接

打开 src/App.js、导入 Link 并添加一些全局导航。注：在本教程中不要对待样式太认真，我们只是为了方便而使用内联样式，你可以根据需要设置样式。

```typescript
import { Link } from "react-router-dom";

export default function App() {
  return (
    <div>
      <h1>Bookkeeper</h1>
      <nav
        style={{
          borderBottom: "solid 1px",
          paddingBottom: "1rem"
        }}
      >
        <Link to="/invoices">Invoices</Link> |{" "}
        <Link to="/expenses">Expenses</Link>
      </nav>
    </div>
  );
}
```

![1668930664190](/images/router/4.png)

![1668930753632](/images/router/5.png)

单击链接和后退/前进按钮。React Router 现在正在控制 URL！

我们还没有在 URL 更改时呈现任何路由，但 Link 可以更改 URL，而不会导致整个页面重新加载。

## 添加一些路由

添加几个新文件：

- `src/routes/invoices.jsx`

- `src/routes/expenses.jsx`

  (文件的位置并不重要，但是当你想要自动生成后端API，服务器渲染，代码分割或者更多的功能时，像这样命名你的文件可以很容易地将这个应用程序移植到其他项目，[Remix](https://remix.run/)😉)

现在在文件中加入以下代码：

expenses.jsx

```
export default function Expenses() {
  return (
    <main style={{ padding: "1rem 0" }}>
      <h2>Expenses</h2>
    </main>
  );
}
```

invoices.jsx

```
export default function Invoices() {
  return (
    <main style={{ padding: "1rem 0" }}>
      <h2>Invoices</h2>
    </main>
  );
}
```

最后，让我们通过在`main.jsx`或者`index.js` 中创建我们的第一个“路由配置”来让 React Router 在不同的 URL 上呈现我们的界面。

```
import { render } from "react-dom";
import {
  BrowserRouter,
  Routes,
  Route
} from "react-router-dom";
import App from "./App";
import Expenses from "./routes/expenses";
import Invoices from "./routes/invoices";

const rootElement = document.getElementById("root");
render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />} />
      <Route path="expenses" element={<Expenses />} />
      <Route path="invoices" element={<Invoices />} />
    </Routes>
  </BrowserRouter>,
  rootElement
);
```

注意：当路由为"/"时它渲染App组件，在"/invoices"时它渲染Invoices组件。

![1668931415821](/images/router/6.png)

![1668931429768](/images/router/7.png)

![1668931443632](/images/router/8.png)

所以到这里我们可以看出，点击后会整页都会变换掉

## 嵌套路由

你可能已经注意到，当点击链接时，“App”中的布局会消失。共享布局是一件令人头疼的事情。我们已经知道，大多数UI都是一系列嵌套布局，这些布局总会映射到URL上，所以这个思路被直接植入到React Router中。

```
import { render } from "react-dom";
import {
  BrowserRouter,
  Routes,
  Route
} from "react-router-dom";
import App from "./App";
import Expenses from "./routes/expenses";
import Invoices from "./routes/invoices";

const rootElement = document.getElementById("root");
render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />}>
        <Route path="expenses" element={<Expenses />} />
        <Route path="invoices" element={<Invoices />} />
      </Route>
    </Routes>
  </BrowserRouter>,
  rootElement
);
```

当路由有子节点时，它会做两件事：

1. 它嵌套了 URL (`"/" + "expenses"` 和 `"/" + "invoices"`)
2. 当子路由匹配时，它将嵌套共享布局的 UI 组件：

但是，为了使（2）生效，我们需要在App.jsx“父”路由中渲染一个

组件。

```
import { Outlet, Link } from "react-router-dom";

export default function App() {
  return (
    <div>
      <h1>Bookkeeper</h1>
      <nav
        style={{
          borderBottom: "solid 1px",
          paddingBottom: "1rem"
        }}
      >
        <Link to="/invoices">Invoices</Link> |{" "}
        <Link to="/expenses">Expenses</Link>
      </nav>
      <Outlet />
    </div>
  );
}
```

现在再次单击。父路由 ( App.js) 仍然存在，而 `<Outlet>` 在两个子路由 (`<Invoices>` 和 `<Expenses>`)之间切换！ 正如我们稍后将看到的，这适用于路由层次结构的任何级别，并且非常强大。

![1668931972695](/images/router/9.png)

![1668931990946](/images/router/10.png)

## 列出发票

通常你会从某个地方的服务器获取数据，但在本教程中，让我们造一些数据，这样我们就可以专注于路由。

创建一个文件src/data.js并将其复制/粘贴到那里：

```js
let invoices = [
  {
    name: "Santa Monica",
    number: 1995,
    amount: "$10,800",
    due: "12/05/1995"
  },
  {
    name: "Stankonia",
    number: 2000,
    amount: "$8,000",
    due: "10/31/2000"
  },
  {
    name: "Ocean Avenue",
    number: 2003,
    amount: "$9,500",
    due: "07/22/2003"
  },
  {
    name: "Tubthumper",
    number: 1997,
    amount: "$14,000",
    due: "09/01/1997"
  },
  {
    name: "Wide Open Spaces",
    number: 1998,
    amount: "$4,600",
    due: "01/27/2998"
  }
];

export function getInvoices() {
  return invoices;
}
```

现在我们可以在发票路由中使用它。让我们也添加一些样式来获得侧边栏导航布局。随意复制/粘贴所有这些，但要特别注意 `<Link>` 组件需要 to 属性：

```
import { Link } from "react-router-dom";
import { getInvoices } from "../data";

export default function Invoices() {
  let invoices = getInvoices();
  return (
    <div style={{ display: "flex" }}>
      <nav
        style={{
          borderRight: "solid 1px",
          padding: "1rem"
        }}
      >
        {invoices.map(invoice => (
          <Link
            style={{ display: "block", margin: "1rem 0" }}
            to={`/invoices/${invoice.number}`}
            key={invoice.number}
          >
            {invoice.name}
          </Link>
        ))}
      </nav>
    </div>
  );
}
```

