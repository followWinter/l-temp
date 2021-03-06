正则匹配的渲染引擎
---
> 一个使用词法分析实现的的简单的前后端通用的模板渲染引擎

### 特性
- 变量渲染
- 条件渲染
- 循环渲染

### 脚本
- `jest`: 测试
- `webpack`: 打包

### 使用示范
```javascript
// 变量渲染
render('{{model.username}}')({username: '111'}) // 111
// 条件渲染
render(`{{ if model.user}}
            <h2>{{ model.user.name }}</h2>
        {{ else }}
            <h2>􏴣名用户</h2>
        {{ endif }}`)({
            user: {
                name: 'me'
            }
        }) // <h2>me</h2>
// for 循环
render(`{{ for model.book }} %>
            <h2>{{index}}:{{value}}</h2>
        {{ endfor }}`)({
            user: {
                book: [1,2,3,4,5,6,7,8,9,10]
            }
        }) // <h2>0</h2><h2>1</h2><h2>2</h2><h2>3</h2><h2>4</h2><h2>5</h2><h2>6</h2><h2>7</h2><h2>8</h2><h2>9</h2>
```

### api
- `render(template:String):Function`: [核心函数]将一个模板字符串转化一个可以多次使用的`compile`函数, 执行该函数才会返回真正的渲染后的模板
   ```
   var compile=render('<h2>{{model.username}}<h2>') // function(model){var template='';template+='<h2>';template+=model.username;template+='';return template;}
   // render 可以只执行一次, 作为模板缓存, 往后直接走 compile 就好了
   compile({username:1111}) // <h2>111</h2>
   ```
- `tokenlize(template:String):Array<Token>`: 将模板解析成`AST`
    ```javascript
    tokenlize(`<p>{{model.name}}</p>`) //[{"type": "string", "value": "<p>"}, {"type": "variable","value": "model.name"}, {"type": "string", "value": "</p>"}]
    ```
- `compile(Array<Token>)`: 将`AST`转化成函数体
    ```javascript
    let tokenlizedArr = tokenlize('{{model.name}}')
    compile(tokenlizedArr) // `var t='';t+=model.name;return t;`
    ```