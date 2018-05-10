<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
  </div>
</template>

<script>
  import StyleEditor from './components/StyleEditor'
  import ResumeEditor from './components/ResumeEditor'
  import ThankEditor from './components/ThankEditor'
  import './assets/reset.css'

  export default {
    name: 'app',
    components: {
      StyleEditor,
      ResumeEditor,
      ThankEditor
    },
    data() {
      return {
        interval: 10,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [
          `/*
* Inspired by http://strml.net/
* 大家好，我是黄小翔。
* 我来写一份简历！
*/

/* 首先给所有元素加上过渡效果 */
* {
  transition: all .1s;
}
/* 白色背景太单调了，我们来点背景 */
html {
  color: rgb(222,222,222); background: rgb(0,43,54);
}
/* 文字离边框太近了 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  width: 45vw; height: 90vh;
}
/* 代码高亮 */
.token.selector{ color: rgb(133,153,0); }
.token.property{ color: rgb(187,137,0); }
.token.punctuation{ color: yellow; }
.token.function{ color: rgb(42,161,152); }

/* 加点 3D 效果呗 */
html{
  perspective: 1000px;
}
.styleEditor {
  position: fixed; left: 0; top: 0;
  -webkit-transition: none;
  transition: none;
  -webkit-transform: rotateY(10deg) translateZ(-100px) ;
          transform: rotateY(10deg) translateZ(-100px) ;
}

/* 接下来我给自己准备一个编辑器 */
.resumeEditor{
  position: fixed; right: 0; top: 0;
  padding: .5em;  margin: .5em;
  width: 48vw; height: 90vh;
  border: 1px solid;
  background: white; color: #222;
  overflow: auto;
}
/* 好了，我开始写简历了 */


`,`
/* 这个简历好像差点什么
 * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
 * 简单，用开源工具翻译成 HTML 就行了
 */
`, `
/* 再对 HTML 加点样式 */
.resumeEditor{
  padding: 2em;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;
  content: counters(section, ".") " ";
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: #ddd;
}
`,`
/*
 * 特别鸣谢。
 */
`, `
/* 再对 HTML 加点样式 */
.thankEditor{
  padding: 2em;
}
.thankEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}
.thankEditor ul,.thankEditor ol{
  list-style: none;
}
.thankEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.thankEditor ol {
  counter-reset: section;
}
.thankEditor ol li::before {
  counter-increment: section;
  content: counters(section, ".") " ";
  margin-right: .5em;
}
.thankEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: #ddd;
}
`],
        currentMarkdown: '',
        fullMarkdown: [
          `黄小翔
====

坐标：江西南昌。

广兰寺，小哥哥，不油腻。现为单身狗一枚。

[下载离线简历](http://www.sitexa.org/anires/static/resume.pdf)

小哥哥技能
====

关于吃的
----
  - 怎么吃都不胖
  - 夏天最爱冰淇淋，冬天最爱泡面

喜欢的人
----
  - 不能比我胖
  - 喜欢狗
  - 喜欢冰淇淋
  - 短发可以，长发也可以
  - 喜欢夏天
  - 可以一起敲代码
  - 不能比我重
  - 不会做饭不要紧
  - 我会做饭
  - 但是不会炒菜
  - 以上只是我内心想一想

关于音乐
----
  - 喜欢网易云
  - 不知道喜欢什么歌(可能是民谣,经常熬夜看评论)

大学生活
----
  - 班长一枚，宣讲员（心理宣讲）
  - 争取拿国家奖学金
  - 苦逼的敲代码
  - 天天满课，下个礼拜开始一周一考
  - 争取脱单
  - 每天三公里，应该可以跑完
  - 食堂经理助理，主要就是处理学生意见
  - 有三个傻逼室友
  - 习惯性忘事情
  - 英语比较差，这个学期一定过四级
  - 还是想脱单

个人技能与评价
----
  - 好像没什么技能
  - 让我想一想
  - 可能长的比较好
  - 室友都嫉妒我
  - 喜欢打羽毛球
  - 但是打不隔壁寝室的那个人
  - 喜欢看书
  - 主要是小黄书之类的
  - 学习一般般
  - 习惯性临时抱佛脚
  - 最好还是觉得长的比较好。。。

工作经历
----

1. 当过家教
2. 卖过加多宝与蓝月亮
3. 去梅岭推销过两天门票，晒得比较黑
4. 进过工厂，去过东莞
5. 今年想去杭州

教育经历
----

1. 兴国平川中学
2. 现在就读东华理工大学 软件工程专业

偶像
----

* 老胡是我偶像
* 为什么呢
* 低调有内涵，重要的是长的帅，演技好，情商高
* [偶像微博](http://www.sitexa.org/technology/%E5%A4%A7%E5%81%A5%E5%BA%B7%E5%95%86%E4%B8%9A%E6%A8%A1%E5%BC%8F.html)
* [偶像图片](http://www.sitexa.org/other/%E7%A4%BE%E5%8C%BA%E7%A4%BE%E4%BA%A4%E5%95%86%E4%B8%9A%E6%A8%A1%E5%9E%8B.html)

链接
----

* [作品下载](https://github.com/sitexa)
* [网易云歌单](http://www.sitexa.org)
* [小黄片](http://www.sitexa.net)
* [小黄书](http://www.sitexa.cn)

联系方式
----

* 电话：18779036002
* 微信：18779036002
* 邮箱：1443690715@qq.com

离线简历
----

[下载简历](http://www.sitexa.org/anires/static/resume.pdf)

`,`
想说的话
----

* 已经是五月份了，不知不觉半年就要过去了。
* 这段时间，我有些诚煌诚恐，无地自容。年前立的fags，做着做着就糊了，真的非常内疚，习惯性幻想，还是需要接受残酷的现实。
* 到底适合学习不，我也不好定位，其实也没得选择。除了学习不会，其他的也不会。只好硬着头皮去学习，主要困惑是概率论，情愿学高数也是不学概率论，应该是说就不应该去学数学。当然
* 自认为学习能力强，追求完美，有点强迫症，爱自言自语，想每科科90分以上，以及代码开发，都追求优美。但都被残酷的现实挤压得很骨感。
* 一直以来，我发现我想要的东西太多了，最后就是什么都没有做好，伴随着小米都要上市了，我一直想我到底要成为什么样的人，我还是没有想明白，但是大学不知不觉已经两年了想说的是专注比广博更有利。
* 对于本专业，一开始是稀里糊涂就选了这个专业，所以肯定是不怎么喜欢这个专业的，更何况那么多人从事这个行业以后都秃顶，内心更是拒绝，但是好像慢慢的喜欢了，可能这就是日久生情吧，反正现在肯定是喜欢上了！
* 对于未来，对于接下来的每一天，其实还是没有想明白我要成为什么样的人，但是我知道需要的是努力，可能努力也不一定会有回报，但是我还是要努力啊！要不然以后怎么娶媳妇，虽然还是单身狗。
* 对于考研，我内心其实还是比较摇摆的，想考研，想去更好的深造，但是我不知道我在怕什么，可能怕失败吧，失败的有阴影了。这个事情暑假在决定吧！
* 叨叨絮絮说了那么多，可能是最近太郁闷了，所以就想吐吐槽，又不知道和谁吐槽。所以就。。。。。。。。
* 最后呢，我要对所有的所有，说一声谢谢，谢谢你们对我的关心和支持！有你们的存在，让我对明天充满希望，对未来充满信心！！
* 当然，每次不开心都不和家里说，怕他们担心，一直想等我以后工作了，就让我爸妈去他们想去的地方看看！

大学课程（不完全）
----

- 高等数学
- 离散数学
- 线性代数
- 概率论与数理统计
- 计算机英语
- C语言
- 面向对象程序设计
- 数据结构与算法设计
- web程序设计
- 计算机组成原理
- 数据库原理
- Java程序设计
- Linux操作系统管理
- Unix系统应用
- JavaScript
- AutoCAD计算机辅助设计
- 安卓智能手机软件开发
- 人机交互界面设计
- 西方经济学
- 管理学
- 经济法
- 会计学原理
- 统计学

`]
      }
    },
    created() {
      this.makeResume()
    },

    methods: {
      makeResume: async function () {
        await this.progressivelyShowStyle(0)
        await this.progressivelyShowResume()
        await this.progressivelyShowStyle(1)
        await this.showResumeHtml()
        await this.progressivelyShowStyle(2)
        await this.progressivelyShowStyle(3)
        await this.progressivelyShowThanks()
      },
      showResumeHtml: function () {
        return new Promise((resolve, reject) => {
          this.enableHtml = true
          resolve()
        })
      },
      progressivelyShowStyle(n) {
        return new Promise((resolve, reject) => {
          let interval = this.interval
          let showStyle = (async function () {
            let style = this.fullStyle[n]
            if (!style) {
              return
            }
            // 计算前 n 个 style 的字符总数
            let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
            let prefixLength = length - style.length
            if (this.currentStyle.length < length) {
              let l = this.currentStyle.length - prefixLength
              let char = style.substring(l, l + 1) || ' '
              this.currentStyle += char
              if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                this.$nextTick(() => {
                  this.$refs.styleEditor.goBottom()
                })
              }
              setTimeout(showStyle, interval)
            } else {
              resolve()
            }
          }).bind(this)
          showStyle()
        })
      },
      progressivelyShowResume() {
        return new Promise((resolve, reject) => {
          let resume = this.fullMarkdown[0]
          let length = resume.length
          let interval = this.interval
          let showResume = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = resume.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              if (prevChar === '\n' && this.$refs.resumeEditor) {
                this.$nextTick(() => this.$refs.resumeEditor.goBottom())
              }
              setTimeout(showResume, interval)
            } else {
              resolve()
            }
          }
          showResume()
        })
      },
      progressivelyShowThanks() {
        return new Promise((resolve, reject) => {
          this.enableHtml = true
          let thanks = this.fullMarkdown[1]
          let length = thanks.length
          let interval = this.interval
          let showThanks = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = thanks.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              if (prevChar === '\n' && this.$refs.thankEditor) {
                this.$nextTick(() => this.$refs.thankEditor.goBottom())
              }
              setTimeout(showThanks, interval)
            } else {
              resolve()
            }
          }
          showThanks()
        })
      }
    }
  }

</script>

<style scoped>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  html {
    min-height: 100vh;
  }

  * {
    box-sizing: border-box;
  }
</style>
