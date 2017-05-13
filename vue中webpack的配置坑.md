#Vue Webpack 配置
```javascript
{
                test: /\.vue$/,
                loader: 'vue-loader',
                options:{
                    //extractCSS: true,
                    loaders:{
                        // css:ExtractTextPlugin.extract({
                        //     use:'css-loader',
                        //     fallback:'vue-style-loader'
                        // })
                        //css:'vue-style-loader!css-loader?importLoaders: 1!postcss-loader',
                    },
                    // postcss: [require('autoprefixer')()]
                }
            },
```

默认 vue-loader 可以处理 css less js html了。不需要再另外写loader。
另外只要有postcss.config.js文件，vue-loader会自动读取，不需要再写。

postcss.config.js 文件 ： 
```javascript
module.exports = {
    plugins: [require('autoprefixer')({browsers: ['last 5 versions']})]
}
```

然后 vue 中 css display：flex 会自动添加前缀。
```css
<style scoped>
.imgclass{
    width:100px;
    height: 100px;
}
ul {
    background-color: red;
    list-style: none;
}
.f{
    display: flex;
}
</style>
```
打包后效果：
```css
.imgclass[data-v-34ce132c]{
    width:100px;
    height: 100px;
}
ul[data-v-34ce132c] {
    background-color: red;
    list-style: none;
}
.f[data-v-34ce132c]{
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
}
```


 如果style标签中再另外引入一个 @import css 。那么生成出来的css 不会自动添加前缀。
```css
<style scoped>
@import './css/flex2.css';
.imgclass{
    width:100px;
    height: 100px;
}
ul {
    background-color: red;
    list-style: none;
}
.f{
    display: flex;
}
</style>
```

flex2.css: 
```css
.flex-div2{
    display: flex;
}
```

打包后效果:

```css
.flex-div2{
    display: flex;
}
.imgclass[data-v-34ce132c]{
    width:100px;
    height: 100px;
}
ul[data-v-34ce132c] {
    background-color: red;
    list-style: none;
}
.f[data-v-34ce132c]{
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
}
```

但是用less就可以 <style lang='less'>, 解决办法，不在style中 @import，在script中 用 import './css/flex2.css' 就可以自动添加前缀了.
```javascript
import './css/flex2.css'
export default {
    data() {
        return {
            msg: 'Hello Vue'
        }
    },
}
```

打包后效果:

```css
.flex-div2{
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
}
.imgclass[data-v-34ce132c]{
    width:100px;
    height: 100px;
}
ul[data-v-34ce132c] {
    background-color: red;
    list-style: none;
}
.f[data-v-34ce132c]{
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
}
```

