# vue-epub-reader

### 非常简单的epub电子阅读器，主要熟悉epubjs的使用

###### ePub电子书解析和渲染
```
// 生成Book对象
this.book = new Epub(DOWNLOAD_URL)
// 通过Book.renderTo生成Rendition对象
this.rendition = this.book.renderTo('read', {
  width: window.innerWidth,
  height: window.innerHeight,
  method: 'default'
})
// 通过Rendtion.display渲染电子书
this.rendition.display()
```

##### ePub电子书翻页
```
// 上一页
function prevPage() {
  if (this.rendition) {
    this.rendition.prev()
  }
}
// 下一页
function nextPage() {
  if (this.rendition) {
    this.rendition.next()
  }
}
```

##### ePub电子书的字号设置和场景切换
```
// 设置主题
function setTheme(index) {
  this.themes.select(this.themeList[index].name)
  this.defaultTheme = index
}
// 注册主题
function registerTheme() {
  this.themeList.forEach(theme => {
    this.themes.register(theme.name, theme.style)
  })
}
// 设置字号大小
function setFontSize(fontSize) {
  this.defaultFontSize = fontSize
  if (this.themes) {
    this.themes.fontSize(fontSize + 'px')
  }
}
```

##### ePub电子书生成目录和定位信息
```
// Book对象的钩子函数ready
this.book.ready.then(() => {
  // 生成目录
  this.navigation = this.book.navigation
  // 生成Locations对象
  return this.book.locations.generate()
}).then(result => {
  // 保存locations对象
  this.locations = this.book.locations
  // 标记电子书为解析完毕状态
  this.bookAvailable = true
})
```

##### ePub电子书通过百分比进行定位
```
function onProgressChange(progress) {
  const percentage = progress / 100
  const location = percentage > 0 ? this.locations.cfiFromPercentage(percentage) : 0
  this.rendition.display(location)
}
```
## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```
