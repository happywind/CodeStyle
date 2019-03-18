# Android-Code-Style-Convention

# Android 命名&代码 参考规范
参考规范，即具备了一定的灵活性，不是说要完全按照这样来写，但是越符合该规范要求对代码的可读性和可维护性可起到积极作用

---

# Java Style
Android开发使用Java语言，因此大部分代码样式都是遵照Java Style，因此不要出现C/C++ Style

- 参考 [Google Java Style](http://google.github.io/styleguide/javaguide.html#s2.3.1-whitespace-characters)

---

# Android Java Style
除了 **Google Java Style** 里面说的大部分外约定外，Android也做了部分约定修改

参考：[Android Code Style Guidelines for Contributors](https://source.android.com/source/code-style.html#java-style-rules)

下面为几个不同于 **Google Java Style** 的地方

## 代码缩进4个字符，语句换行缩进8个字符
```java
if (condition) {
····body()；// 4个字符
}

Instrument i =
········someLongExpression(that, wouldNotFit, on, one, line); // 8字符

```

## 属性字段命名约定

- 非公共的，非全局的字段命名以 m 开头
- 静态的static 的字段，s 开头
- 其他字段小写开头
- 公共的静态final字段，public static final，即常量，全部大写下划线


```java
public class MyClass {
    public static final int SOME_CONSTANT = 42; // 常量写法
    public int publicField;
    private static MyClass sSingleton;
    
    // 成员变量写法
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

*补充*：**但是**，开发中我们为了接收服务器传回来的Json数据，Android项目结构中多见于model，domain包下面数据类型，以及DTO，我们这里不按照 **属性字段命名约定**，而是类似下面代码

```java
public class User {
	private String name;
	private Integer age;
	private String gender;
	private Boolean isAdmin;
}
```

## Android 判断语句样式
请使用下面两种:

```java
if (condition) {
	body();
}

if (condition) body();
```
---

# Java Style 上比较重要，或常忽略的地方
- 文件编码，统一 utf-8
- 所有的声明缩进，换行缩进全部用空格 space，不用 tab。IDE里可以设置
- 代码列宽一般控制在 80 到 100 个字符长度
- 包导入 Import 的优化，例如顺序，不要导入百搭型。一般使用IDE可以优化 Import，Eclipse快捷键应该是ctrl(cmd)+shift+o，Android Studio类似
- 垂直方向上的空行，主要是用于逻辑分组：

	```java
	private TextView mName;
	private TextView mAge;
										// 垂直空行
	public String name;
	public int age;
										// 垂直空行
	public void createUser() {
		// .....
	}
	
	```
- 一个变量，一个声明，举例为是不好的方式 `int a, b; // bad`

	```java
	int a; // good
	int b;
	```
- javadoc的使用，尽量给 public/protected 的方法加上 javadoc
	
	```java
	/**
	* This is a javadoc
	*/
	public void method() {}
	
	```
- 静态常量，大写下划线：`public static final int SOME_CONSTANT = 42;`

# 代码块化
一个方法，或一段代码段，保持块状，并且尽量不要在一个方法里写的过多，充分将功能分块

```java
main { //例子
	int a;
	int b = 3;
	int c = 4;
	
	if (b > c) { // 需要抽出来
		a = b;
	} else {
		a = c;
	}
	
	.....
	.....
}

转变为：
main {
	int a;
	int b = 3;
	int c = 4;
	a = max(b, c);
	.....
	.....
}

max(int a, int b) {
	if (b > c) {
		a = b;
	} else {
		a = c;
	}
}

```
---

# 项目结构、包名

appId：全局包名，com.example

- com.example.app // 一般存放Application、AppSharedPreference、Constants
- com.example.activity // 可以单独这样给 activity 做一个包，

or

- com.example // 把Application、AppSharedPreference、Constants放最外层也行
- 或者把所有 activity 放最外层，Application等 还是放 com.example.app

### 以下包名只是参考，同意、类似的即可
- com.example.fragments
- com.example.webservice/com.example.web // 网络请求相关
- com.example.adapter
- com.example.db // 数据库相关
- com.example.model // 所有 models，DTO
- com.example.utils // 所有工具类包
- com.example.widget // 自定义控件包
- com.example.service 
- com.example.animation 

### 包名下面还可以建子包
例如: com.example.webservice.request \ com.example.webservice.response

# XML 文件命名约定
- activity_
- fragment_
- row_ // 自定义ListView的Item，偶尔见到有使用 item_
- dialog_ 
- layout_ // 重用布局时候

# XML 文件里面View的Id约定
很多时候项目中并没有对id的命名有特殊约定，所以常见会有三种：

1. 通用，直接根据控件要代表的事物取名，`@+id/name` `@+id/card_list`
2. 当一个布局文件里面如果控件多了，我们会需要IDE帮我们先做一些查找，这个时候可以加上类型前缀，`@+id/tv_name` `@+id/lv_cards`
3. 还有的会加上 Activity 的前缀用于区分控件所属 Activity，`@+id/activity_login_tv_account` `@+id/activity_login_btn_login`

**实际情况下，一个Activity只会去找setContentView()后的View，因此**

- 1 - 在控件不多的时候比较好 
- 2 - 控件多了可以让IDE先帮忙筛选一些 
- 3 - 筛选的更加精确，并可以区别大致所在位置，但是命名太过繁琐

*很多情况下 2 就可以满足我们需要，但是以所有人约定为准*
>
- Button - btn
- EditText - et
- TextView - tv
- Checkbox - chk
- RadioButton - rb
- ToggleButton - tb
- Spinner - spn
- Menu - mnu
- ListView - lv
- GalleryView - gv
- LinearLayout -ll
- RelativeLayout - rl

# 资源文件命名
可以采用 <类型> _ <控件类型> _ <意义>.png 形式

eg.
 
```java
ic_back.png
bg_btn_back.png
```

# Java 成员变量名约定
并没有特别的约定，可以直接用mBack、mBackBtn、mBackButton 类似的形式
如果比较在意控件类型，或者说控件同意非同类，偏向使用后面两种 mBackButton, mBackTextView

*如果不习惯使用m前缀（最好遵守）大家约定好什么形式就用什么形式，保持统一，这个就是约定的作用*
