## 代码规范

###  集成.editorconfig配置在根目录

[EditorConfig](https://editorconfig.org/)有助于为不同IDE上处理同一个项目的多个开发人员维护一致的编码风格

```
# .editorconfig

root = true

[*] # 表示所有文件适用
charset = "utf-8" # 设置文件字符集为utf-8
indent_style = space # 缩进风格（tab | space）
indent_size = 2 # 缩进大小
end_of_line = lf # 控制换行符类型（lf-unix风格(\n) ｜ cr-windows风格(\r) ｜ crlf-mac风格(\r\n)）
trim_trailing_whitespace = true # 去除行首的任意空白字符
insert_final_newline = true # 始终在文件末尾插入一个新行

[*.md] # 表示仅 md 文件适用以下规则
max_line_length = off
trim_trailing_whitespace = false
```

vscode需要安装一个插件：EditorConfig for VS Code

### 使用prettier工具

prettier是一款强大的代码格式化工具

1. 安装prettier

   ```shell
   npm install prettier -D
   ```

2. 配置.prettierrc文件

   * useTabs：是否使用tab缩进，否则使用空格缩进
   * tabWidth：tab是空格的情况下，使用几个空格代表一个tab
   * printWidth：一行代码的最大长度，超出则多行显示
   * singleQuote：是否使用单引号，false使用双引号
   * trailingComma：在多行输入的尾逗号是否添加
   * semi：末尾是否要加分号

   ```json
   {
     "useTabs": false,
     "tabWidth": 2,
     "printWidth": 80,
     "singleQuote": true,
     "trailingComma": "none",
     "semi": true
   }
   ```

3. 创建.prettierignore忽略文件

   ```
   /dist/*
   .local
   .output.js
   /node_modules/**
   
   **/*.svg
   **/*.sh
   
   /public/*
   
   ```

4. 在package.json的scripts中配置执行命令

   `"prettier": "prettier --write ."`

   这样就可以在命令行中运行`npm run prettier`对除了忽略文件之外的所有文件进行格式化了

5. vscode中可以安装prettier插件，这个插件会根据你的配置文件进行格式化的

6. 如果和eslint冲突了，那么重启vscode试试

###  使用ESLint检测

使用eslint来检测代码是否规范

1. vscode中需要安装eslint插件

2. 解决eslint和prettier冲突问题：

   安装插件`npm i eslint-plugin-prettier eslint-config-prettier -D`

   ```js
   // .eslintrc.js
   module.exports = {
     root: true,
     env: {
       node: true
     },
     extends: [
       "plugin:vue/vue3-essential",
       "eslint:recommended",
       "@vue/typescript/recommended",
       "plugin:prettier/recommended" // 和prettier配置一致，这样不会有冲突
     ],
     parserOptions: {
       ecmaVersion: 2020
     },
     rules: {
       "no-console": process.env.NODE_ENV === "production" ? "warn" : "off",
       "no-debugger": process.env.NODE_ENV === "production" ? "warn" : "off"
     }
   }
   ```

总的来说，EditorConfig、Prettier 和 ESLint 都有助于提高代码的质量和一致性，但它们的作用不同：EditorConfig 用于设置文件格式，Prettier 用于格式化代码，ESLint 用于检查代码。您可以将这三个工具结合起来使用，以达到更好的效果。

### git Husky和eslint

husky是一个git hook 工具，可以帮助我们触发git提交的各个阶段：pre-commit、commit-msg、pre-push，我们可以自定义在这些阶段需要进行哪些操作，一旦触发这些阶段，我们所定义的操作就会执行。比如我们需要在pre-commit的时候进行代码格式的校验，并且格式化代码，就可以在pre-commit的hook中运行lint或者prettier

可以一步到位进行安装运用：

```shell
npx husky-init && npm install
```

这里我们要知道npx是npm包的运行器，可以在还没安装某个包的时候直接执行安装包的命令工具，比如上面的代码，我们在还没安装husky的时候运行了husky包的husky-init命令，紧接着再安装

上面的命令会做以下三件事，所以我们也能自己手动做：

1. 安装husky相关的依赖

   ```shell
   npm i husky -D
   ```

2. 在项目目录下创建.husky文件夹

   里面可以进行hook的配置，在这里写好我们需要在特定hook进行的操作

3. 在package.json中添加脚本

   ```json
   "scripts": {
   	"prepare": "husky install"
   }
   ```

### git commit规范

1. 代码提交风格

   * Commitizen是一个帮助我们编写规范commit message的工具

     1. 安装Commitizen

        ```shell
        npm install commitizen -D
        ```

     2. 安装cz-conventional-changelog，并且初始化cz-conventional-changelog:

        ```shell
        npx commitizen init cz-conventional-changelog --save-dev --save-exact
        ```

        这个命令会帮我们安装cz-conventional-changelog

        并且在package.json中进行配置

     3. 使用：`npx cz`

        ```
        // 可以在package.json的scripts中进行配置
        
        "commit": "cz"
        
        // 运行
        npm run commit
        ```

        * 第一步选择type，本次更新的类型
          * feat：新增特性(feature)
          * fix: 修复Bug(bug fix)
          * docs：修改文档(documentation)
          * style：代码格式修改
          * refactor：代码重构(fefactor)
          * perf：改善性能(A code change that improves performance)
          * test：测试
          * build：变更项目构建或外部依赖
          * ci：更改持续集成软件的配置文件和package中scripts命令
          * chore：变更构建流程或辅助工具
          * revert：代码回退
        * 第二步择本次修改的范围(作用域)
        * 。。。。。

   2. 代码提交验证

      * 通过commitlint来限制提交信息的格式规范

        1. 安装@commitlint/config-conventional和@commitlint/ci

           ```shell
           npm i @commitlint/config-conventional @commintlint/ci -d
           ```

        2. 在根目录创建commitment.config.js文件，配置commitlint

           ```js
           module.exports = {
           	extends: ['@commitlint/config-conventional']
           }
           ```

        3. 使用husky生成commit-msg文件，在提交commit信息的时候进行验证

           ```shell
           npx husky add .husky/commit-msg "npx --no-install commintlint --edit $1"
           ```

           

   