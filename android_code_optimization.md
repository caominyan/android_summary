android 优化相关
##1.布局优化
利用 GPU Overdraw 查看过度绘制
1. 去除底层非必要的background 颜色设置
2.减少布局的嵌套 合理使用RelativeLayout LinearLayout
3.自定义View的时候，通过canvas的clipRect帮助系统识别可视区域
4.对某些不是必要布局用ViewStub进行加载
5.按需要使用merge 或者 include标签引入布局
6.用draw9patch 将不显示区域使用透明，利用Android 2D渲染器对.9进行UI优化
7.少用wrapcontent（会增加布局的measue计算）
8.删除控件的无用的属性

##2.启动优化
1.Application 按需加载某些sdk初始化
2.不在onpause 中进行比较耗时的操作，比如文件处理（包括shareperence 操作），如果有必要放在onStop里进行
3.分布式延期加载数据，比如首页里多个fragment的数据进行懒加载
4.后台避免有高的CPU线程运行，能停止的就停止

##3.内存优化
1.不要用sharefrence 存储大数据结构，特别是json文件，key 和 value 尽量简单的存储，
把不频繁修改的和频繁修改的sharefrence分开存储，以免经常IO操作
2.对资源对象进行及时回收 cursor file及时close
3.特别注意使用观察者模式的对象引用，需要及时取消注册
4.对某些静态引用需要进行回收清理，特别对于Collection 内的对象，记得设置成null
5.可以不用非静态内部类的，尽量用静态内部类代替，特别是thread handler这些具有延时特性，会持有外部类的引用，可以使用弱应用引用外部类进行使用
6.方案1:webview 最好不要写在xml，文件里，通过动态添加方式加载，在离开webview时，去除父布局对webview的引用，并且调用destroy，
方案2:承载webview的Activity 在独立进程，离开时直接删除进程，完成资源释放
7.需要H5对网页进行一些调整，把js业务代码放在dom下面，这也会使得android 的webview显示的更加快速，把某些不变化的js库合并到sdk里，利用webviewClient shouldInterceptRequest 用本地文件替换服务端的资源文件
8.在调用比较频繁的方法里注意对象的创建，以免产生内存抖动，比如自定义view的onDraw方法。
9.注意能使用基本数据类型不要使用装箱的数据类型，以及注意字符串的拼接
10.尽量不使用枚举值，编译器会为枚举值生成实例，导致dex文件变大，而且枚举天生是单例模式，所以如果枚举引用了大对象，将产生内存泄漏，所以尽量使用int值来替换枚举值

11.用momery profile 或者 MAT 或者 用 leakcary 检查内存使用情况，修复潜在的内存泄漏问题

##4.稳定性优化
1.利用Android Lint 和 findbugs 查找代码潜在缺陷







