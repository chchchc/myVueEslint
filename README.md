# Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

## Recommended IDE Setup

- [VS Code](https://code.visualstudio.com/) + [Vue - Official](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (previously Volar) and disable Vetur

## Vue3+Vite 项目，如何配置 ESLint 和 Prettier 实现代码规范化

### ESLint

1.node 版本是 18.17.1

2，安装 eslint 有快速开始模式和手动安装两种方法，这里我选择手动安装，如下

--2.1 npm install eslint@8.48.0 eslint-plugin-vue@9.17.0 -D

--2.2 项目根目录中添加一个.eslintrc.js 配置文件，文件内容如项目中所示。

--2.3 然后在项目根目录添加.eslintignore 文件, 忽略不想让 ESLint 检查的文件夹和文件，文件内容如项目中所示。

--2.4 打开 VS Code, 在 Extensions 中搜索 ESLint, 找到之后点击 install, 启用 ESLint 后会寻找到项目根目录下的.eslintrc.js 配置文件, 根据里边的规则对项目代码进行检查

### Prettier

1.安装 Prettier，npm install prettier@2.8.8 -D

2.创建一个.prettierrc.json 配置文件，让编辑器和其他工具知道您正在使用 Prettier，文件内容如项目中所示。

--2.1 使用 Prettier 格式化所有文件：npx prettier --write .,故意打乱文件中格式，执行该命令，文件被格式化，说明生效了。

--2.2 VS Code 安装 Prettier，点击 install

3.在项目根目录添加.vscode 文件夹并且新建一个 settings.json 文件，配置保存时使用 Prettier 对代码进行格式化，文件内容如项目中所示。

4.当 ESLint 的规则和 Prettier 的规则相冲突时，就会发现一个尴尬的问题，用 Prettier 来格式化代码，ESLint 就会报错。我们的目标是要让 ESLint 使用 Prettier 的规则去检查代码语法和风格等问题。解决如下：

--4.1 安装 eslint-plugin-prettier 和 eslint-config-prettier

---4.1.1 ：npm install eslint-plugin-prettier@4.2.1 eslint-config-prettier@8.10.0 -D

--4.2 您需要在.eslintrc.js 中添加 plugin:prettier/recommended 作为最后一个扩展：文件内容如项目中所示。

注意：修改 prettier 配置后需要重新启动 VS Code 编辑器 ESLint 才能够正常工作
需要装的依赖见 package.json

### 问题

1. eslint 校验不生效，报错如下：
   Instead rename .eslintrc.js to end in .cjs, change the requiring code to use dynamic import() which is available in all CommonJS modules, or change "type": "module" to "type": "commonjs" in G:\a-my test-vue\my-vue-app\package.json to treat all .js files as CommonJS (using .mjs for all ES modules instead).

原因：<https://article.juejin.cn/post/7244070072787501112>
解决：.eslintrc.js 文件重命名为 .eslintrc.cjs

2. eslint --ext .vue,.js,.ts,.jsx,.tsx 和 eslint --ext .js,.vue src，为什么后者 eslint 校验生效，前者不生效，两者有啥区别，区别在于后者是校验 src 目录下的文件。

3. eslint 校验生效是需要指定文件参数的：npx eslint --ext .vue,.js,.ts,.jsx,.tsx . 这样也是生效的，注意一个.结尾，代表文件参数
   见：<https://eslint.nodejs.cn/docs/latest/use/command-line-interface>

## lint-staged + husky

1. 安装 husky: npm install --save-dev husky

2. 执行：husky init，会在项目根目录下生成一个 .husky 文件夹，里面有一个 pre-commit 文件，package.json 增加了一个 prepare 脚本，prepare 脚本会在每次 npm install（不带参数）之后自动执行。 也就是说当我们执行 npm install 安装完项目依赖后会执行 husky 命令，该命令会创建.husky/目录并指定该目录为 git hooks 所在的目录。

2.1 可以创建新的钩子：echo "npm test" > .husky/pre-commit，会在 pre-commit 文件夹写入 npm test。

3. 安装 lint-staged：npm add -D lint-staged。

4. 在 package.json 配置 lint-staged 所需要的规则,见文件所示。

5. 执行命令：echo "npx lint-staged" > .husky/pre-commit，在 pre-commit 中创建了一个钩子。

6. 执行提交命令：git commit -m "test"测试一下，报错：.husky/pre-commit: .husky/pre-commit: cannot execute binary file
   husky - pre-commit script failed (code 126)

   6.1 报错原因：Adding a hook is as simple as creating a file. This can be accomplished using your favorite editor, a script or a basic echo command. For example, on Linux/macOS:就是说这个命令是适合 Linux/macOS 的，我们写项目时是用 windows+vs code，其 vs code 的终端在 wins 下是使用 powershell 的，但是 echo 在 unix/mac 和 wins 中的使用是有差异的。因此如果想在 wins 下使用这条命令创建添加钩子文件，可以在 git bash 环境下使用，因为 git bash 使用的是类似 Unix 的环境

7. 随便写一个文件,不符合 eslint 规则的，然后执行 git commit -m "test"测试一下，能拦截掉就代表成功了。
