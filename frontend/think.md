# 实现一个demo.mov视频中的组件

## 要求

- 实现 demo.mov 中的效果
- 封装为 `<Carousel>` 组件
- 使用 React Hooks 实现，不能用 Class Component
- 使用 TypeScript 实现


## 加分项

- 单元测试

## 分析需求

- 视频中是一个简单的走马灯的组件或者叫幻灯片。
- 封装的难点是2个动画效果，一个是每一个页面的滚动效果，预估过渡动画是1秒，另外一个是下面的dots，在每一页停留的时候有个组件变白的效果，预估时间是3秒。因为没有具体的设计稿，所以大部分的视觉效果按照看到的看着做。
- 前端做动画基本就是2种方式
    - 利用css的transition，keyframe等，通过变换dom的class来实现动画。好处就是动画效果比较稳定，也无需关心js执行异常等。不方便的就是回调函数和时间点无法很有效的把控。
    - 利用js方法周期性的改变dom的属性以此达到动画的效果。常用的周期调用的函数就是setTimeout，setInterval，requestAnimationFrame。此种处理的好处就是比较方便的掌控动画的过程和时间点，可以在不同时刻执行回调函数，方便操作。不便就是需要自己控制异常，书写大量的辅助函数，用以控制动画过程。
- 然后我得夸一下出题者，为了考察应试者的能力考虑，故意提供了高度相同（1800px)而宽度不一的图片，看下应试者如何处理。我也是在写代码的时候才发现这一点。我觉得挺好的，前端也得学会处理此类问题，当然工作中的做法就是找设计师处理了，为了尊重出题者的良苦用心用心，我还是用设置背景色的方式实现了，但是问题就是只对背景单一的图片适用。
- 至于写test，确实很少写，很多工程师尤其是前端，不喜欢写test，我也是，惭愧。所以就简单的写了一个test，就是判断一下，dom是否存在。复杂的test也不太会写。

## 扩展需求

- 视频中的展示效果还是有比较多的可以完善的地方，比如边缘展示的问题，视频效果是滚动到最后一张，再重新从第一张开始滚动，视觉效果过于闪烁。只是有三张的情况还好，如果有6张呢？10张呢？从第10张直接滚动到第一张，这个视觉看着就不完美了。
- 像是走马灯组件，除了自动右滚之外，一般可以设置从左边滚动，展示最后一张，层叠展示就像扑克牌的展示效果。还可以垂直方向展示。
- 一般在展示的时候，会copy第一张放到队尾，copy最后一张，放到第一张之前，这样，边界的滚动看起来就很自然了。
- 一般来讲，作为组件应该可以有多个参数，让外部用户可以参与组件的控制。比如是否自动播放，滚动方向，dots自定义，滚动时间（duration）。

## 定方案

- 虽然有很多改进的地方，但是毕竟是一个审核性考试，所以也就不用较真的去实现所有的考虑。为了尊重出题者，还是要按照要求去做。
- 在时间效率之下，尽量少用外部组件，还要兼顾简洁和效果，决定把动画部分封装即可。使用react-slick

## 吐槽

- 声明，此处只是作为一个工程师自我认知，不喜可喷
- 对于typescript的定位和作用实在不敢恭维。不否定他的优势和作用。但是也不是说用了ts就显得高大上，一些经典的前端组件都是原生书写。不知道什么开始，年轻的工程师开始炫技，好像ts就高大上，js就很土一样。殊不知很多优秀的前端组件都是在jQuery时代完成的。包括现在很多的所谓的react组件库，vue组件库，很多都是jQuery组件的封装。就说react-slick这个组件就是jQuery slick的react封装。好的是软件的设计和实现方式，而不是ts还是js。
- 另外一个对于ts的不悦就是，利用后端的类型思想完全打破了js的优势。js之所以快速发展，其本身特点有很大的作用。ts完全限制这点，把js楞生生改变成强类型语音，加入接口，加入泛型等等后端的概念。我之前写java的，就是喜欢js的放荡不羁才写前端，ts深深伤害了我的骄傲。亦或者ts可以继承发展js像c++之于c，再或者ts作为独立语言被推广，我都认可。现在ts还是要编译成js不是，还有大部分工程师在开发界面的时候（非组件或者nodejs），使用ts的优势和意义呢？大部分前端工程师，在使用ts时候，总结的方法就是any yyds。
- 针对出题者的要求，【- 使用 React Hooks 实现，不能用 Class Component】，我尊重出题者的考虑。但是我觉得没有必要，如果你是想知道面试者会不会用hook，我觉得这就是有点不信任现在的工程师了，hook又不是很难的东西，即使没用过也不妨，看看就会。如果出题者，认为用hook比class高大上，那么我这里就简单吐槽一下，官方在出hook的说明中，并没有说hook一定比class有优势，才出的hook，只是照顾到编程习惯，照顾函数式编程才考虑使用hook，而且大部分hook，class都有对应的实现方法。并不是一个全新的东西。而且hook在diff的时候，存在一些隐患，并且对于深层对象的effect效果并不如class setState。useState和useEffect的滥用，也会使得很多代码变的复杂。举个例子，多个参数的情况（3个以上），就存在多种监听组合，导致useEffect钩子变多，如果直接把参数用数组[arg1,arg2,...]包起来都监听，那么未必比一个setState简洁。毕竟我不用声明了，只要在改变state的func里面执行就好了。而且支持object嵌套。
- 官方在出现fiber的版本是16.0.0，这个时候是class的使用方式，而在16.0.8的时候才出现的hook，并且在以后的多个大版本中对于hook并没有特别大的升级。也就是说，官方也不是为hook是论的。我个人认为工程师需要对于语言，框架等有自己的判断和认知，哪怕错了，可以在不断的学习中完善和进步，而不是一味去追新。尤其是一些所谓的互联网大厂，为了绩效而工作的工程师，往往带歪业界。