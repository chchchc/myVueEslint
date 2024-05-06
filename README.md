# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Vue - Official](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (previously Volar) and disable Vetur

## Vue3+Vite 项目，如何配置 ESLint 和 Prettier 实现代码规范化

### ESLint

1.node 版本是 18.17.1
2，安装 eslint 有快速开始模式和手动安装两种方法，这里我选择手动安装，如下
2.1 npm install eslint@8.48.0 eslint-plugin-vue@9.17.0 -D

2.2 项目根目录中添加一个.eslintrc.js 配置文件，文件内容如项目中所示。
2.3 然后在项目根目录添加.eslintignore 文件, 忽略不想让 ESLint 检查的文件夹和文件，文件内容如项目中所示。

2.4 打开 VS Code, 在 Extensions 中搜索 ESLint, 找到之后点击 install, 启用 ESLint 后会寻找到项目根目录下的.eslintrc.js 配置文件, 根据里边的规则对项目代码进行检查

### Prettier

1.安装 Prettier，npm install prettier@2.8.8 -D  
2.创建一个.prettierrc.json 配置文件，让编辑器和其他工具知道您正在使用 Prettier，文件内容如项目中所示。
2.1 使用 Prettier 格式化所有文件：npx prettier --write .,故意打乱文件中格式，执行该命令，文件被格式化，说明生效了。

2.2 VS Code 安装 Prettier，点击 install

3.在项目根目录添加.vscode 文件夹并且新建一个 settings.json 文件，配置保存时使用 Prettier 对代码进行格式化，文件内容如项目中所示。

4.当 ESLint 的规则和 Prettier 的规则相冲突时，就会发现一个尴尬的问题，用 Prettier 来格式化代码，ESLint 就会报错。我们的目标是要让 ESLint 使用 Prettier 的规则去检查代码语法和风格等问题。解决如下：
4.1 安装 eslint-plugin-prettier 和 eslint-config-prettier
4.1.1 ：npm install eslint-plugin-prettier@4.2.1 eslint-config-prettier@8.10.0 -D
4.2 您需要在.eslintrc.js 中添加 plugin:prettier/recommended 作为最后一个扩展：文件内容如项目中所示。

注意：修改 prettier 配置后需要重新启动 VS Code 编辑器 ESLint 才能够正常工作
需要装的依赖见 package.json

### 问题

1. eslint 校验不生效，报错如下：
   Instead rename .eslintrc.js to end in .cjs, change the requiring code to use dynamic import() which is available in all CommonJS modules, or change "type": "module" to "type": "commonjs" in G:\a-my test-vue\my-vue-app\package.json to treat all .js files as CommonJS (using .mjs for all ES modules instead).

原因：<https://article.juejin.cn/post/7244070072787501112>
解决：.eslintrc.js 文件重命名为 .eslintrc.cjs

2. eslint --ext .vue,.js,.ts,.jsx,.tsx 和 eslint --ext .js,.vue src，为什么后者 eslint 校验生效，前者不生效，两者有啥区别，区别在于后者是校验 src 目录下的文件。

3.eslint 校验生效是需要指定文件参数的：npx eslint --ext .vue,.js,.ts,.jsx,.tsx . 这样也是生效的，注意一个.结尾，代表文件参数
见：<https://eslint.nodejs.cn/docs/latest/use/command-line-interface>
