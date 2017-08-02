# cssmodules-thirdpart-components

css modules 在第三方嵌入方向组件化开发过程中的应用

### 避免样式冲突

css modules最直接的优点，避免全局样式冲突，在第三方嵌入方向意义显得更加重大，因为第三方嵌入方向除了受自身的影响之外，还要受宿主页面css的影响。


但是组件化开发过程中难免会有依赖的问题，譬如a组件依赖b组件，想要和b组件共享样式

### 方案一

global

```
    :global(.title) {
        color: #ff0;
    }
```
构建之后

```
    .title{
        color: #ff0;
    }
```
使用global不会应用hash命名，相当于将样式直接暴露在了全局，因此不能在这种类型的应用中使用

`方案一不可取`

### 方案二

composes


```
    // face.styl
    .className {
        color: #f00;
    }
    // index.styl
    .otherClassName {
      composes: className from "./face.styl";
      width: 200px;
    }
```
// 使用

```
    import styles from './index.styl';
    ...
        render() {
            return(
                <div className={styles.otherClassName}></div>
            )
        }
    ...
```

构建后
```
    // css
    .className-xxxlui1 {
        color: #f00;
    }
    .otherClassName-xxxmkwh {
        width: 200px;
    }
    // dom
    <div class="className-xxxlui1 otherClassName-xxxmkwh"></div>
```

优点：适应于有样式依赖的时候，能制定要依赖的样式，灵活好用。
缺点：但是对于批量依赖不能很好的满足，只能一个一个去定义
### 方案三
import
```
// base.styl
.wrap {
    width: 500px;
}

// index.styl
@import './base.styl';
```

使用
```
    import styles from 'index.styl';
    ...
        render() {
            return(
                <div className={styles.wrap}></div>
            )
        }
    ...
```

引用了base的样式之后，在引用的页面可以直接通过`styles.className`来使用
优点：在base.styl中定义的样式都可以使用
缺点：会将base.styl中定义的样式都复制一份到当前样式中。如果`base.styl`有多处使用，则里面的样式会经过不同的hash命名生成多个供不同的模块使用

### 继续思考
针对方案二的不能批量处理的缺点，咱们可以考虑结合预处理器的编程能力来达到批量处理的目的







