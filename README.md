##### 1、把含小数点的数字字符串转换为int类型

```
String num ="100.00";
int abc =Double.valueOf(num).intValue();      //  转换为int类型
```


##### 2、数字序列化

```
public class GenSerialize {
	// DecimalFormat 是 NumberFormat 的一个具体子类，用于格式化十进制数字
	private static NumberFormat f = new DecimalFormat("0000000000");
	public static String genSerialize(int i) {
		return f.format(i);
	}
	
	public static Integer getNum(String str) {
		return Integer.parseInt(str);
	}
	
	public static void main(String[] args) {
		for (int i = 1; i <= 80000; i++) {
			System.out.println(genSerialize(i));
			System.out.println(getNum(genSerialize(i)));
		}
	}
}
```

##### 3、Java判断字符串是否为数字(包括浮点类型)


```
public static boolean isNumber(String str){
    String reg = "^[0-9]+(.[0-9]+)?$";
    return str.matches(reg);
}
```
##### 4、JList监听，选择会得到两次事件解决

```
public void valueChanged(ListSelectionEvent ls){
    if(!ls.getValueIsAdjusting()){
        String selection = (String)list.getSelectedValue();
    }
}
```

> 鼠标按下的时候会触发事件，鼠标放开的时候又会触发一次事件。<br/>判断event.getValueIsAdjusting()，只有第一次鼠标按下的时候才会返回true

##### 5、关于list元素去重问题解决

```
List<String> list = new ArrayList<String>();
list.add("taozi");
list.add("taozi");
list.add("Tom");
...
Set<String> set = new HashSet<String>(list);
List<String> listTrans = new ArrayList<String>(set);
```
##### 6、获取浏览器名称及版本号

```
<dependency>
	<groupId>eu.bitwalker</groupId>
	<artifactId>UserAgentUtils</artifactId>
	<version>1.20</version>
</dependency>
```


```
public String login(HttpServletRequest request, HttpServletResponse response, HttpSession session)
			throws IOException {
		// 获取浏览器信息
		Browser browser = UserAgent.parseUserAgentString(request.getHeader("User-Agent")).getBrowser();
		// 获取浏览器版本号
		Version version = browser.getVersion(request.getHeader("User-Agent"));
		String info = browser.getName() + "/" + version.getVersion();
		return info;
}
```
##### 7、将数组中的零放到末尾
```
// 测试主类
public static void main(String[] args) {
	int[] arr = new int[] { 2, 1, 0, 0, 5, 0, 0, 7 };
	printArray(arr);
	int[] result = removeMiddleZero(arr);
	printArray(result);
}

// 移除数组中间0值
public static int[] removeMiddleZero(int[] arr) {
	int cursor = 0;
	for (int i = 0; i < arr.length; i++){
		if (arr[i] != 0) {
			arr[cursor] = arr[i];
			cursor++;
		}
	}
	for (int i = cursor; i < arr.length; i++) {
		arr[i] = 0;
	}
	return arr;
}

// 打印数组
public static void printArray(int[] arr) {
	for (int i = 0; i < arr.length; i++) {
		System.out.print(arr[i] + " ");
	}
	System.out.println();
}
```

##### 8、中文字符判断
```
private static boolean isChinese(char c) {
	Character.UnicodeBlock ub = Character.UnicodeBlock.of(c);
	if (ub == Character.UnicodeBlock.CJK_UNIFIED_IDEOGRAPHS
			|| ub == Character.UnicodeBlock.CJK_COMPATIBILITY_IDEOGRAPHS
			|| ub == Character.UnicodeBlock.CJK_UNIFIED_IDEOGRAPHS_EXTENSION_A
			|| ub == Character.UnicodeBlock.CJK_UNIFIED_IDEOGRAPHS_EXTENSION_B
			|| ub == Character.UnicodeBlock.CJK_SYMBOLS_AND_PUNCTUATION
			|| ub == Character.UnicodeBlock.HALFWIDTH_AND_FULLWIDTH_FORMS
			|| ub == Character.UnicodeBlock.GENERAL_PUNCTUATION) {
			return true;
	}
	return false;
}

public static boolean hasChinese(String strName) {
	char[] ch = strName.toCharArray();
	for (int i = 0; i < ch.length; i++) {
		char c = ch[i];
		if (isChinese(c)) {
			return true;
		}
	}
	return false;
}
```
- 正则表达式匹配中文字符
```
public static boolean isChinese(String str) {
	Pattern p = Pattern.compile("[\u4e00-\u9fa5]");
	Matcher m = p.matcher(str);
	if (m.find()) {
		return true;
	}
	return false;
}
```

##### 9、数组扩容
```
public static Object capacityArray(Object array, int increment) {
	Class<?> clazz = array.getClass();
	if(clazz.isArray()) {
		Class<?> componentType = clazz.getComponentType();
		int length = Array.getLength(array);
		Object newArray = Array.newInstance(componentType, length + increment);
		System.arraycopy(array, 0, newArray, 0, length);
		return newArray;
	}
	return null;
}
```

##### 10、数字格式化货币字符串
> 格式化对象可以指定语言环境，在Java中使用Local类的对象来表示，在该类中包含了各种语言环境。
```
NumberFormat format = NumberFormat.getCurrencyInstance(Locale.CHINA);
System.out.println("Locale China : " + format.format(1000));
format = NumberFormat.getCurrencyInstance(Locale.US);
System.out.println("Locale US : " + format.format(1000));
```

#### 11、异或运算符用于两个操作数互换数值
> 异或运算符是用符号“^”表示的，其运算规律是：<br/>
> 两个操作数的位中，相同则结果为0，不同则结果为1。

```
int a = 10;
int b = 20;
a = a ^ b;
b = b ^ a;
a = a ^ b;
System.out.println("a=" + a);
System.out.println("b=" + b);
```

#### 12、适配器类使用
```
public MyFrame(String title) {
	setTitle(title);
	setSize(400, 300);
	addWindowListener(new WindowAdapter() {
			
		@Override
		public void windowClosing(WindowEvent e) {
			System.out.println("windows closed and app is closed ...");
			System.exit(0);
		}
			
	});
}

public static void main(String[] args) {
	new MyFrame("Windows closed!").setVisible(true);
}
```

##### 13、获取当前日期
```
// 可以方便地修改日期格式
Date now = new Date();
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
String nowStr = dateFormat.format(now);
System.out.println(nowStr);

// 可以对每个时间域单独修改
Calendar c = Calendar.getInstance();

int year = c.get(Calendar.YEAR);
int month = c.get(Calendar.MONTH) + 1;
int date = c.get(Calendar.DATE);
int hour = c.get(Calendar.HOUR_OF_DAY);
int minute = c.get(Calendar.MINUTE);
int second = c.get(Calendar.SECOND);

System.out.println(year + "/" + month + "/" + date + " " + hour + ":" + minute + ":" + second);
```

##### 14、String转换成byte[]或者byte[]转换成String
```
// Original String
String string = "hello world";

// Convert to byte[]
byte[] bytes = string.getBytes();	// 或者getBytes("utf-8")
 
// Convert back to String
String s = new String(bytes);
```

##### 15、计算三角形面积
```
double a = 3, b = 4, c = 5;
double p = (a + b + c) / 2;
double val = p * (p - a) * (p - b) * (p - c);
double area = Math.sqrt(val);
System.out.println("area = " + area);
```

##### 16、格式化数字展示形式
```
double val = 190089.87;
DecimalFormat df = new DecimalFormat("#,###.00"); 
String format = df.format(val);
System.out.println("format val : " + format);
```

##### 17、Java调用dll的例子
```
// 定义接口Testdll，继承自com.sun.jna.Library
public interface Testdll extends Library {
	
	// msvcrt为dll名称, msvcrt目录的位置为:C:\Windows\System32下面
	Testdll instance = (Testdll) Native.loadLibrary((Platform.isWindows() ? "msvcrt" : "c"),
			Testdll.class);
	
	// printf为msvcrt.dll中的一个方法
	void printf(String format, Object... args);
}

public static void main(String[] args) {
	// 调用printf打印信息
	Testdll.instance.printf("Hello JNA!");
}
```
```
<dependency>
	<groupId>net.java.dev.jna</groupId>
	<artifactId>jna</artifactId>
	<version>4.0.0</version>
</dependency>

<dependency>
	<groupId>net.java.dev.jna</groupId>
	<artifactId>jna-platform</artifactId>
	<version>4.0.0</version>
</dependency>
```

##### 18、二分查找

> 1. 必须采用顺序存储结构
> 2. 必须按关键字大小有序排列

```
public static int binarySearch(int[] arr, int key) {
    int low = 0;
    int high = arr.length - 1;

    while (low <= high) {
	int mid = (low + high) / 2;
	if (key < arr[mid]) {
		high = mid - 1;
	} else if (key > arr[mid]) {
		low = mid + 1;
	} else {
		return mid;
	}
    }
    return -1;
}
```

##### 19、Java递归删除一个目录下文件和文件夹
```
// Java递归删除一个目录下文件和文件夹
public static void deleteDir(File dir) {
    if (dir.isDirectory()) {
        String[] children = dir.list();
	System.out.println(children);
	// 递归删除目录中的子目录下
	for (int i = 0; i < children.length; i++) {
		deleteDir(new File(dir, children[i]));
	}
    }
    dir.delete();
}
```

##### 20、计算程序运行时常
```
long start = System.currentTimeMillis();
...
long end = System.currentTimeMillis();
System.out.println("程序运行时间 : " + (end - start) / 1000 + " s");
```
