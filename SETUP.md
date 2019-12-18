- 采用 [nvm](https://github.com/creationix/nvm#installation) (macOS/Linux) 或 [nvm-windows](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) 安装和管理 Node。

- 采用 [VS Code](https://code.visualstudio.com) 和 [Chrome Debugger Extension](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) 开发。

---

## Create React App

- `npx create-react-app my-app` / `yarn create react-app my-app`

## Add EditorConfig, ESLint, Stylelint, Prettier

- 安装依赖：`yarn add eslint stylelint prettier --dev`
- 安装依赖：`yarn add eslint-config-airbnb eslint-config-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks --dev`
- `.gitignore`，配置 Git 忽略项
- `.editorconfig`，配置编辑器
- `.eslintrc.json`、`.eslintignore`，配置 ESLint，查找和修复 JavaScript 代码的问题
- `.prettierrc.yml`、`.prettierignore`，配置 Prettier ，用以 Git 提交时自动格式化代码

## Add Pre-commit Hook

- `npx mrm lint-staged`，Git hooks
- `yarn add imagemin-lint-staged --dev`

`package.json` 配置：

```json
"husky": {
  "hooks": {
    "pre-commit": "lint-staged",
    "post-commit": "git update-index --again"
  }
},
"lint-staged": {
  "src/**/*.{js,jsx}":               [ "eslint --fix", "git add" ],
  "src/**/*.css":                    [ "stylelint --fix", "git add" ],
  "src/**/*.scss":                   [ "stylelint --fix --syntax=scss", "git add" ],
  "src/**/*.less":                   [ "stylelint --fix --syntax=less", "git add" ],
  "src/**/*.{json}":                 [ "prettier --write", "git add" ],
  "src/**/*.{md,html}":              [ "prettier --write", "git add" ],
  "src/**/*.{png,jpeg,jpg,gif,svg}": [ "imagemin-lint-staged", "git add" ]
}
```

## Adding Styles

限定样式作用域，采用 CSS Modules：样式文件为 `[name].module.css`，打包之后样式名为 `[filename]\_[classname]\_\_[hash]`；

在项目中引用样式示例：

```javascript
import styles from './Button.module.css'; // 导入 CSS Modules 样式
import './another-stylesheet.css'; // 导入常规样式表

class Button extends Component {
  render() {
    return <>
      <button className={styles.error}>Error Button</button>
      <button className="error">Error Button</button>
    </>;
  }
}
```

## Adding Images, Fonts, and Files

在项目中引用静态资源示例：

```javascript
import logo from './logo.png'; // Tell Webpack this JS file uses this image
import { ReactComponent as Logo } from './logo.svg';

const App = () => (
  <div>
    <img src={logo} alt="Logo" />
    {/* Logo is an actual React component */}
    <Logo />
  </div>
);
```

## Import antd

- 引入 UI 框架 Ant Design `yarn add antd`
- 添加 Less 样式预处理 `yarn add less less-loader --dev`
- 按需加载组件代码和样式 `yarn add babel-plugin-import --dev`
- 修改 Create React App 内部配置 `yarn add react-app-rewired customize-cra --dev`

创建 `config-overrides.js` 文件并配置：

```javascript
const { override, fixBabelImports, addLessLoader } = require('customize-cra');

module.exports = override(
  fixBabelImports('import', {
    libraryName: 'antd',
    libraryDirectory: 'es',
    style: true,
  }),
  addLessLoader({
    javascriptEnabled: true
  }),
);
```

配置 `package.json` 文件：

```json
"scripts": {
  "start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
  "eject": "react-app-rewired eject"
},
```

- ``，