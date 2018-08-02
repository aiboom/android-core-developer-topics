## Intent是什么？

Intent是一个消息传递对象，促成任意应用组件的通信操作。

## Intent的主要功能有哪些？

Intent的主要功能有如下三个方面：首先是启动Activity，然后是启动Service，还有一个是传递Broadcast。

## Intent有哪些类型？

Intent分为两种类型，分别是显式Intent和隐式Intent。两者的区别是前者会指定接收Intent的组件，而后者则仅声明接收Intent的组件需要满足的条件。

## Intent过滤器是什么，有什么作用？

Intent过滤器是用来描述应用组件可以接收的Intent满足的条件的（隐式Intent）。Intent过滤器可以在代码中实现，不过大部分情况下其在manifest文件中声明。

## 启动Service为什么要是显式Intent？

这是出于安全的考虑，如果使用隐式Intent启动服务，用户将无法看到那些服务被启动。从Android5.0开始，使用隐式Intent启动Service将引发异常。

## 一个Intent通常由那些组件构成？

一个Intent通常由如下组件构成：Component，Action，Data & Type，Category，Extras，Flag。

## 如何理解Intent的Component组件？

Intent的Component组件用于指定接收该Intent的组件名称的，所以一个Intent有没有Component是判断该Intent的类型的唯一标准。

Intent的Component组件是一个ComponentName对象，可以通过`setComponent()`，`setClass()`，`setClassName()`或Intent的构造函数来指定Component。

需要注意的是，一个Intent只能有一个Component组件。

## 如何理解Intent的Action组件？

Intent的Action组件携带了Intent的行为信息，用来表示该Intent想要实现的操作。Action组件将很大程度上决定了Intent接下来的组件的构成，特别是数据和extra中包含的内容。

Intent的Action组件分为两种，一种是系统界别的Action，这类Action都表示了一个唯一的操作，另一种是用户自定义的Action，开发者基于此可以定制自己需要的操作。

Action以字符串形式表示，通常使用`setAction()`或Intent构造函数来指定操作。一个Intent只能有一个Action。

## 如何理解Intent的Data & Type组件？

当Intent的Action组件确定了Intent想要实现的操作后，接下来就需要Intent提供具体的操作数据和/或数据类型。比如一个Intent想要实现的操作是查看一个pdf文档，那么该Intent就需要提供想要查看的pdf文档的相关信息。也就是它的URI以及MIME_TYPE。

Intent的Data必须是Uri格式，指定MIME_TYPE也对Android筛选接收Intent的最佳组件十分有帮助。不过有时通过Uri本身的结构就能判断出MIME_TYPE，因此是否指定MIME_TYPE可以根据实际情况来定。

如果只需要设置Data，就使用`setData()`，如果仅需要设置Type，就使用`setType()`，注意两者要同时设置，就使用`setDataAndType()`。切记不要同时使用`setData()`和`setType()`，两者会相互抵消彼此的值。

## 如何理解Intent的Category组件？

Intent的Category组件为接收Intent的应用组件添加了额外的约束。不过一般自定义的Intent使用不到它，而系统发送的Intent将更多的使用Category组件，因此Category组件出现在Intent Filter中的频率比较高。

可以使用`addCategory()`来添加Category，一个Intent可以有多个Category。同时Category也能自行定义。

## 如何理解Intent的Extra组件？

Intent的Extra组件携带了完成请求操作的附加信息的键值对。可以使用`putExtra()`方法向Intent中添加一个键值对，或使用`putExtras()`向Intent中添加一个包含多个键值对的Bundle对象。

## 如何理解Intent的Flag组件？

在Intent中定义的，充当Intent元数据的标志。多用于限定启动的Activity的行为。使用`setFlags()`可以为Intent添加单个或多个标志。

## 如何避免隐式启动Activity时由于找不到合适的Activity而引发崩溃？

```java
// Create the text message with a string
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
sendIntent.setType("text/plain");

// Verify that the intent will resolve to an activity
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(sendIntent);
}
```

## 如何强制使用应用选择器？

```java
Intent sendIntent = new Intent(Intent.ACTION_SEND);
...

// Always use string resources for UI text.
// This says something like "Share this photo with"
String title = getResources().getString(R.string.chooser_title);
// Create intent to show the chooser dialog
Intent chooser = Intent.createChooser(sendIntent, title);

// Verify the original intent will resolve to at least one activity
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(chooser);
}
```

## 如何理解Intent过滤器？

Intent过滤器用来过滤发送过来的Intent请求，看应用组件是否满足接收该Intent请求的要求。

## 如何避免其他应用由于知道自己的应用组件名而强制显式启动的行为？

在manifest.xml中将组件的`exported`属性设置为false，即能避免其被应用外的Intent启动。

## PendingIntent是什么？和Intent有何不同？

理解PendingIntent并不难，实际上，PendingIntent可以看做是对Intent进行了一层包装，使Intent请求可以异步操作。这是什么意思呢？传统的Intent传递过程，Intent的请求操作是实时反馈的，Intent请求发送出去后，系统就立即返回结果。而PendingIntent包装的Intent请求，如果此时没有符合处理请求的组件，此Intent会被保存起来，当某个时间触发该Intent的条件满足，这个Intent就会被调出来使用。

所以不要认为PendingIntent和Intent的区别像网上说的一个立即执行一个稍后执行。这种说法值考虑了一种情况，实际上PendingIntent包装的Intent也能立即执行。

## Intent过滤器的过滤原则是怎样的？
