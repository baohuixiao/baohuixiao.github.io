* namespace

  其实没软用，现在有模块化了，它的概念就像是将原本的函数、变量，放到一个新创建的对象中一样，然后通过这个对象的名称访问

* .d.ts文件

  它是用来做类型声明(declare)，它仅仅用来做类型检测，告诉typescript我们有哪些类型

  * 内置类型声明

    typescript自带的，下载typescript的时候一起下载了，内置了javascript运行时的一些标准化api的声明文件

  * 外部定义类型声明

    一般指第三方库的类型声明

    1. 将声明文件写在库里，一起载下来
    2. 通过社区的一个公有库Definitely Typed下载到对应库的类型声明，这是别人帮忙写好的

  * 自己定义类型声明

    ```typescript
    // 声明一个模块
    declare module 'lodash' {
    	// 给此模块声明一个方法
      export function join(arr: any[]): void
    }
    ```

    