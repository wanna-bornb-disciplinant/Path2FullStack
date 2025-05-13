### 学习之前的初印象

* 作为我自己学习网站开发的一个小总结 目前的前端开发非常复杂 工具和框架繁多 入手会感觉混乱 根据GPT的描述 目前的网站开发主要是前端基本是React和Vue二选一 后端基本上是Flask\Django或Node.js+Express二选一

* 最终我确定的学习全栈是：  
    前端：React用于构建用户界面，TypeScript添加类型系统，添加一部分的tailwind CSS构建界面  
    后端：Flask+Django  
    通信方式：REST API或GraphQL实现前后端的数据交互

* 开发前的环境准备阶段需要安装Node.js和npm/yarn、python和pip、虚拟环境工具venv或virtualenv、vscode和相关插件(安装完Node.js后需要完全重启一次vscode vscode才能使用相关命令)

* vscode的相关插件需要安装的是：  
  ES7+ React/Redux/React-Native snippets  
  Tailwind CSS IntelliSense  
  Prettier - Code formatter  
  ESLint  
  npm Intellisense  
  Path Intellisense  
  具体每个插件的作用还不清楚 先学着看吧
 
* 即使后端选择了Flask，但在开发前为什么还是需要安装Node.js和npm/yarn：   
  开发工具在后台运行的时候都是依赖于Node.js环境的，另外npm/yarn这些前端的包管理器也依赖于Node.js
  
* 这里搬一些对这几种框架和工具的解释：  
  JavaScript 是一种编程语言，最初只运行在浏览器中；Node.js是一个JavaScript运行时环境，它让JavaScript可以在浏览器之外运行，尤其是在服务器端。  
  React、Vue、Express 这些框架/库在开发过程中，通常都需要 Node.js 环境来运行相关工具链，比如打包、开发服务器、模块管理等，但Express 是专为 Node.js 开发的后端框架，React 和 Vue 是前端框架，它们本身不依赖于 Node.js，
  但现代前端开发（如打包、热更新等）通常会用到基于 Node.js 的工具链（如 npm、webpack、vite）

### 简单的一个实验：

* 打开vscode->终端->输入以下命令：
  ```bash
  # 创建项目主目录
  mkdir my-fullstack-app
  cd my-fullstack-app
  
  # 创建后端目录
  mkdir backend
  cd backend
  python -m venv venv
  
  # 激活虚拟环境
  # Windows:
  venv\Scripts\activate
  # macOS / Linux:
  source venv/bin/activate
  
  # 安装 Flask
  pip install flask flask-cors
  
  # 创建 Flask 主文件
  code app.py
  ```
* 在app.py文件中粘贴以下内容(后端)：
  ```python
  from flask import Flask, jsonify
  from flask_cors import CORS
  
  app = Flask(__name__)
  CORS(app)  # 允许跨域请求
  
  @app.route("/api/hello")
  def hello():
      return jsonify(message="Hello from Flask!")
  
  if __name__ == "__main__":
      app.run(debug=True)
  ```
* 在vscode中新开一个终端，回到项目根目录(前端的flask是运行在虚拟环境下的，因此运行前端时必须出虚拟环境重启一个终端)：
  ```bash
  cd ..
  npm create vite@latest frontend -- --template react-ts
  cd frontend
  npm install
  npm run dev
  ```
* 修改前端文件：
  ```bash
  code frontend/src/App.tsx
  ```
  将其内容改为：
  ```tsx
  import { useEffect, useState } from 'react'
  import './App.css'
  
  function App() {
    const [message, setMessage] = useState('Loading...')
  
    useEffect(() => {
      fetch('http://127.0.0.1:5000/api/hello')
        .then(res => res.json())
        .then(data => setMessage(data.message))
        .catch(err => setMessage('Error connecting to backend'))
    }, [])
  
    return <h1>{message}</h1>
  }
  
  export default App
  ```
* 启动前后端：  
  后端在虚拟环境下和backend目录下：
  ```bash
  python app.py
  ```
  前端在frontend目录下：
  ```bash
  npm run dev
  ```
  此时访问`https://localhost:5173`会显示：
  ```csharp
  Hello from Flask!
  ```

### 对小实验的初步思考和认识：
* 为什么最终访问的地址是 *https://localhost:5173* :  
  实际上搭建react项目时，*npm run dev*就启动了一个本地开发服务器，上述的地址就是前端页面运行的地址
* flask_cors起到了什么作用：  
  *flask_cors*允许前端的请求跨域访问后端接口，跨域指的是当前所在页面的网页请求了另一个不同源的地址，网址的源由三部分组成：协议、域名\IP、端口号，只要有一个不一样，都算是跨域
* localhost是一个什么地址：  
  默认是一个回环地址，为127.0.0.1，这个地址表示服务永远发生在本地，跟互联网没关系，例如React调用的端口号是5173，Flask调用的端口号是5000
* 还有一些常见的问题：
  基于react和vue开发的前端还会有类似之前H5+CSS出现的html文件吗？
  ![问题1]()
  
