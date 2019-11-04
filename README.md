## License:

MIT

# iOS_Emoji
emoji for iOS

Chinese:
  更多详情请点击：[http://www.真无聊.com](http://xn--rgvu79ah1g.com/ios%E4%B9%8B%E8%B7%AF/iOS-Android-Server-Emoji)

## 本文件会随着iOS系统的更新而更新，欢迎star。
  
English:
  more infomation:[http://www.真无聊.com](http://xn--rgvu79ah1g.com/ios%E4%B9%8B%E8%B7%AF/iOS-Android-Server-Emoji)
  
  
# 前言

关于emoji，有的同学会很奇怪，为什么要特意写这个，原生的本来不就支持了emoji输入以及显示吗？的确，原生的控件都支持emoji的显示以及输入。但是有的需求一旦进入了开发阶段，你就会发现并不是那么简单的一件事情。

Server端：

	由于server一开始并不支持emoji，现在你post含有emoji的数据到server，server会出现异常，主要是server的mysql开始的时候没有定义4位的字段，varchar都是3位的(普通的字符串或者表情都是占位3个字节，所以utf8足够用了，但是移动端的表情符号占位是4个字节，普通的utf8就不够用了，为了应对无线互联网的机遇和挑战、避免 emoji 表情符号带来的问题、涉及无线相关的 MySQL 数据库建议都提前采用 utf8mb4 字符集，这必须要作为移动互联网行业的一个技术选型的要点)。

Android端：

	大家都知道，Android和iOS的表情显示是不一样的(Android的更萌，不信去试试)，据说Android N的推出使Android的emoji更加像iOS的了。图不同编码相同倒不是什么问题，致命的是我们需要做得和微信那样，emoji表情保持一致性，而且Android里面的输入法(例如搜狗)键盘显示的是iOS样式的(emoji最原始样式)，想一下如果用户输入的和看到的是不一样的会是什么样的体验？所以我们这里必须拿emoji出来处理！而且和iOS一样需要做过滤处理！这里的过滤处理主要是过滤掉iOS8.3推出的肤色。但是本文提到的Android库里面包含了肤色，所以如果Android需要显示肤色的话，可以不用过滤，对了，就算你可以显示带肤色的emoji，你是输入不了的，因为Android的输入法不支持输入带肤色的emoji。更多处理请看下面。

iOS端：

	别以为iOS端自带了emoji就轻松，因为需要做和server端，Android的一致性，我们这里需要将emoji转化为共用的字符(下文会知道他们是以两个“:”来定义的例如 :emoji:)，除开兼容，我们还要做emoji的过滤！emoji的列表不是一成不变的！在不同的版本苹果都会推出新的emoji，也就是说emoji是动态的！参考微信朋友圈的实现我们就知道，在一些低版本的设备中不支持高版本的emoji的话微信会直接过滤掉！所以我们这里也需要做过滤处理！更多处理请看下面。

## 走过的弯路

###### 参考文献：
iOS & Android:

解决方案：

<https://github.com/arvida/emoji-cheat-sheet.com/>

<https://github.com/quxionglie/emoji-convertor>

<http://blog.csdn.net/qdkfriend/article/details/7576524>

<http://www.cnblogs.com/YungMing/p/5080437.html>

emoji过滤：（用于缺失定义的emoji）

<http://www.pigg.co/emoji-filter.html>

### iOS:

<http://jeason.gitcafe.io/blog/iOS+Anroid+Emoji/>

<http://cenalulu.github.io/linux/character-encoding/>

emoji编码范围<http://blog.csdn.net/liujinlongxa/article/details/44207003>

### Android:
[https://github.com/rockerhieu/emojicon](https://github.com/rockerhieu/emojicon)

### Server
升级数据库<https://segmentfault.com/a/1190000000616820>

用于前端网页显示emoji<https://github.com/pepibumur/emojize>

### emoji列表：
<http://apps.timwhitlock.info/emoji/tables/unicode>

[http://www.emoji-cheat-sheet.com/](http://www.emoji-cheat-sheet.com/)

[https://en.wikipedia.org/wiki/Emoji](https://en.wikipedia.org/wiki/Emoji)

[http://punchdrunker.github.io/iOSEmoji/table_html/index.html](http://punchdrunker.github.io/iOSEmoji/table_html/index.html)

### 工具库：(gitHub上star最多的)

#### iOS：

[https://github.com/valeriomazzeo/NSString-Emoji](https://github.com/valeriomazzeo/NSString-Emoji)

[https://github.com/diy/NSStringEmojize/](https://github.com/diy/NSStringEmojize/)

#### Android：
[https://github.com/rockerhieu/emojicon](https://github.com/rockerhieu/emojicon)

#### Mac OS X 查看所有emoji图片的方法：

快捷键： control + command + 空格

弹出如下图

![emoji列表图](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/ios_andorid_server_emoji_list.png)

点击左上角设置按钮-->自定义列表--->勾选Unicode--->点击完成。

就可以看到完整emoji列表和对应的Unicode码或者UTF-8码了。

遇到的问题：iOS端从iOS8.3开始出现的emoji肤色

# 最终的解决方案
Android：

使用工具：

[https://github.com/rockerhieu/emojicon](https://github.com/rockerhieu/emojicon)

安卓的同学比较懒，没有写文字的习惯。故这里我仅贴出工具。


iOS：

使用工具库:

[https://github.com/valeriomazzeo/NSString-Emoji](https://github.com/valeriomazzeo/NSString-Emoji)

注意：

1. 这个工具里面的emoji字符表不全，需要自己新增一些进去。
2. 这个工具里面没有iOS8.3开始Apple新增的emoji肤色选择！
3. 没有使用另外一个cheat-emoji官方工具的原因是官方工具只是将“:test:”等字符的转化为emoji，但是却缺少了将emoji反转成“:test:”的方法！！。想想也是醉了！

我这里给出肤色对应的编码：

{% highlight objective-c linenos %}

        //by Vbon haha
        //here is emoji color unicode sinces iOS8.3
        //default is not containt color unicode which look like yellow color sinces iOS8.3
        //Before iOS8.3 emoji is no color unicode containt and look like white color.
        ([[UIDevice currentDevice].systemVersion floatValue] >= 8.3 ? @"🏻" : @""): @":skinColor1:",//white             --> Unicode: U+1F3FB
        ([[UIDevice currentDevice].systemVersion floatValue] >= 8.3 ? @"🏼" : @""): @":skinColor2:",//white&littleBrown --> Unicode: U+1F3FC
        ([[UIDevice currentDevice].systemVersion floatValue] >= 8.3 ? @"🏽" : @""): @":skinColor3:",//white&deepBrown   --> Unicode: U+1F3FD
        ([[UIDevice currentDevice].systemVersion floatValue] >= 8.3 ? @"🏾" : @""): @":skinColor4:",//brown             --> Unicode: U+1F3FE
        ([[UIDevice currentDevice].systemVersion floatValue] >= 8.3 ? @"🏿" : @""): @":skinColor5:" //black             --> Unicode: U+1F3FF				
{% endhighlight %}  
                                               
注意一点：emoji的不断新增！

iOS&Android的同学注意了！

由于emoji会不断新增，也就是说emoji的Unicode范围会一直增大！所以需要做一下处理，对于不在自己代码定义的范围内的emoji需要过滤掉！例子：微信朋友圈。

小道消息称Unicode协会将会在明年新增多150个emoji，具体时间还没定。新增表情对于大家的使用时很爽！(例如iOS9后多了个中指的表情)。但是对于我们程序本身来说就意味着必须要做emoji字库和对应的key不全的问题的处理！就算以后他新增1000个我也无所谓了！因为我是按照微信的做法（直接忽略没有的emoji，不信你试试在iOS端发🙃这个emoji，去Android设备看看，你就知道了，微信是直接过滤掉的！）。下面我直接点，贴代码，大家直接拿去用。

注意：这里的代码是结合上面的工具来用的。

Android：

工具：<https://github.com/rockerhieu/emojicon>

修改工具里面的编码：

![Android](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/ios_andorid_server_emoji_android.png)

安卓的同学需要特别注意两点

1. 这个emoji库是通过不断对比map里面的数据来查询的，会导致在列表中的多emoji时滚动会很卡。解决办法：在得到列表json数据的时候，在映射的时候转码成emoji，并将该emoji直接放入实体。这样就避免了滚动list的时候转码带来的卡顿！
2. 在输入emoji的地方(发表内容)，连续输入多个emoji会卡顿甚至卡死，原因同上，因为这个库是对比查找的，默认的算法是假如有1000个emoji，那么就1个emoji的key对比1000次，而输入的时候由于每次输入都进行了转码，所以输入一个emoji就要对整段text进行emoji对比。性能严重下滑！解决办法是改变自己的算法，只转码新输入的那个字符。
3. 总的来说就是特殊场景特殊处理，看自己的解决问题的能力了！

iOS：

工具：<https://github.com/valeriomazzeo/NSString-Emoji>

修改工具里面的编码：

![Android](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/ios_andorid_server_emoji_ios.png)


需要注意的几种情况：

1. 上传字符串前需要将字符串使用工具转码。 
2. 用户输入字符串的时候看情况是否需要转码。
3. 如果有字数限制的！注意emoji转码带来的字数问题哟！

安卓的同学注意了！上面推荐的工具是十分强大的！里面包括了iOS的肤色emoji。需要和iOS端沟通处理。例如我这里用skinColor1-5来区分除了默认颜色(白种人)的之外的5中颜色。安卓也是需要定义这几种编码的！用于过滤或者显示不同肤色！


（更新：2016.5.12）

测试今天发来了bug，说是有的字符没过滤掉。我看了一下的确，在开源的库中没有添加进去一些iOS9.3新的emoji。而且值得注意的是：新增的emoji中竟然出现了新的[emoji语法]！！！新增了同性之间的emoji和家庭的emoji有着不同的语法！一个emoji可能有很多个编码。

例如：“👩‍❤️‍👩” 这个emoji表示的是女女之间的同志关系。

你会发些这一类emoji竟然连控制台也打印不出来！(我的系统是最新的10.11.4)。

![](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/1.png)

![](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/3.png)

打开emoji列表，我们可以看到他的编码语法。

![](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/2.png)

总共由6个4字节的编码组成！比以前的最多3个4字节编码翻了一倍！

细心的同学可以在上面控制台的图片里面发现一些端倪，上面6个4字节组成的实际上可以用已经有的emoji进行翻译！像“👩‍❤️‍👩”翻译过来就是:woman::heart::woman:，看是不是通俗易懂？至于中间的“？”号我们可以看回emoji列表里面的字节是什么，可以知道的是“U+200D”这样的东西。这样就好办了，我们可以将它转换成“\U0000200D”，那样程序就知道了。由于安卓没有这样新增的图片我们只好过滤掉这样的东西了。在文件中新增key-value如下：

![](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/4.png)

这样就可以完美过滤了。

我在新增iOS9.3 emoji的过程中发现文件中的emoji国旗没有多少支持。

![](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/5.png)

对比系统emoji列表可以在文件中新增进去。

![](http://xn--rgvu79ah1g.com/images/ios_andorid_server_emoji/6.png)

这样就可以将app的emoji支持到最新的iOS9.3了。

PS：iOS 13.2兼容支持。

为了大家方便我将文件开源到这里：
<https://github.com/vbonluk/iOS_Emoji>

文件随着系统升级及时更新，欢迎star。谢谢

# Last：
本以为emoji是个很简单的东西。没想到真正弄起来的时候需要考虑的东西还是有点多的！server端，Android端，iOS端。走过的弯路远不止上面列出的链接。上面的链接只是找到了比较有用的给大家参考。
文章是后来才写的，中间一些细节可能忘记了。😚

原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com

友情链接联系:5914018@qq.com

交流QQ群：271568188
