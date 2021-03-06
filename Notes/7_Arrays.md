# 数组
数组是值的有序集合。每个值叫做一个元素，而每个元素在数组中有一个位置，以数字表示，称为索引。Javascript数组是无类型的：数组元素可以是任意类型，并且同一个数组中的不同元素也可能有不同的类型。数组的元素甚至也可能是对象或其他数组，这允许创建复杂的数据结构。Javascript数组是的索引是基于零的32位数值。Javascript数组是动态的，根据需要会增长或缩减。

Javascript数组是Javascript对象的特殊形式，数组索引实际上和碰巧是整数的属性名差不多。数组的实现是经过优化的，用数字索引来访问数组元素一般来说比访问常规的对象属性要快很多。

## 7.1 创建数组
使用数组直接量是创建数组最简单的方法，在方括号中将数组元素用逗号隔开即可。如：

```javascript
var empty = [];
var primes = [2, 3, 5, 7, 11];
var misc = [1.1, true, "a", ];
```

数组直接量中的值不一定要是常量，它们可以是任意的表达式：

```javascript
var base = 1024;
var table = [base, base+1, base+2, base+3];
var b = [[1, {x:1, y:2}], 2];
```

如果省略数组元素的某个值，省略的元素将被赋值为undeinfed：

```javascript
var count = [1, , 3];		// length: 3
var undefs = [, , ,];		// length: 2
```

调用构造函数Array()是创建数组的另一个方法，有以下三种形式：

```javascript
var a = new Array();		// 调用时没有参数，空数组，等同于直接量[]
var a2 = new Array(10);		// 调用时使用1个数组参数，指定数组长度
// 显示指定两个或多个数组元素或者数组的一个非数值元素
var a3 = new Array(5, 4, 3, 2, 1, "abc");	// [5, 4, 3, 2, 1, "abc"]
```

## 7.2 数组元素的读和写
使用[]操作符来访问数组中的元素。数组的引用位于方括号的左边。方括号中是一个返回负整数值的任意表达式。该语法可以读写数组的一个元素。如：
```javascript
var a = ["furzoom"];		//
var value = a[0];			// read 
a[1] =3.14;					// write
i = 2;
a[2] = 3;					// write
a[i + 3] = ".com";			// write
```

数组是对象的特殊形式。Javascript将指定的数字索引值转换成字符串，然后将其作为属性名来使用。对常规对象也是这么处理。数组的特别之处在于，当使用小于2^32的非负整数作为属性名时数组会自动维护其length属性值。使用负整数或者非整数来索引数组，它就只能当做常规对象属性。数组索引仅仅是对象属性名的一种特殊类型。数组可以从原型中继承元素。在ECMAScript 5中，数组可以定义元素的getter和setter方法。

## 7.3 稀疏数组
稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的length属性值代表数组中元素的个数。如果数组是稀疏的，length属性值大于元素的个数。可以用Array()构造函数或简单地指定数组索引值大于当前数组长度来创建稀疏数组。如：

```javascript
a = new Array(5);			// 数组没有元素，但a.length是5
a = [];						// lenght = 0
a[1000] = 0;				// a.lenght是1001
```

足够稀疏的数组通常在实现上比稠密的数组更慢，内存利用率更高，在这样的数组中查找元素的时间和常规对象属性的查找时间一样长。注意，在数组直接量中省略值时不会创建稀疏数组。省略的元素在数组中是存在的，其值为undefined。这和数组元素根本不存在是有一些微妙的区别的(在新版chrome中已无区别)，可以用in操作符检测两者之间的区别：

```javascript
var a1 = [,,,];
var a2 = new Array(3);
0 in a1;					// true
0 in a2;					// false
```

## 7.4 数组长度
每个数组都有一个length属性，就是这个数组使其区别于常规的Javascript对象。针对非稀疏数组，lenght属性值代表数组元素中元素的个数。当数组是稀疏数组时，length属性值大于元素的个数。如果为一个数组元素赋值，这经索引i大于或等于现有数组的长度时，length属性的值将设置为i+1。当设置数组的lenght属性为一个小当前长度的非负整数n时，当前数组中那些索引值大于或等于n的元素将从中删除。

```javascript
var a = [1, 2, 3];
a.length;				// 3
a.length = 2;
a[2];					// undefined
```

在ECMAScript 5中可以用Object.defineProperty()让数组的length属性变成只读的，如：

```javascript
a = [1, 2, 3];
Object.defineProperty(a, "length", {writable: false});
a.length = 0;			// a.length是3
```

此时也不能给数组添加元素了。

## 7.5 数组元素的添加和删除
给数组元素添加数组的最简单的方法就是给新索引赋值，也可以使用push()方法在数组末尾增加一个或多个元素：

```javascript
a = [];
a[0] = 1;
a.push(2, 3);		// [1, 2, 3]
```

在数组尾部压入一个元素与给数组a[a.length]赋值是一样的。可以像删除对象属性一样使用delete运算符来删除数组元素：

```javascript
a = [1, 2, 3];
delete a[1];
1 in a;				// false
a.length;			// 3 delete不影响数组长度
```

对一个数组元素使用delete不会修改数组的length属性。如果从数组中删除一个元素，它就变成稀疏数组。

## 7.6 数组遍历
使用for循环是遍历数组元素最常见的方法：

```javascript
var keys = Object.key(o);
var values = [];
for(var i = 0; i < keys.length; i++) {
	var key = keys[i];
	valeus[i] = o[key];
}
```

可以对循环进行优化，使得对数组长度的查询只执行一遍：

```javascript
for(var i = 0, len = keys.length; i < len; i++) {
	// 循环体不变
}
```

这些例子假设数组是稠密的，并且所有的元素都是合法数据。否则，使用数组元素之前应该先检测它们。如果想排除null、undefined和不存在的元素，代码如下：

```javascript
for(var i = 0; i < a.length; i++) {
	if (!a[i]) continue;
	// 循环体
}
```

如果只想跳过undefined和不存在的元素，代码如下：

```javascript
for(var i = 0; i < a.length; i++) {
	if (a[i] === undefined) continue;
	// 循环体
}
```

如果只想跳过不存在的元素而仍然要处理存在的undefined元素，代码如下：

```javascript
for(var i = 0; i < a.length; i++) {
	if (!(i in a)) continue;
	// 循环体
}
```

还可以使用for/in循环来处理稀疏数组，不存在的索引将不会遍历到：

```javascript
for(var index in a) {
	var value = a[index];
	// 循环体
}
```

由于for/in循环能够枚举继承的属性名，如添加到Array.prototype中的方法。由于这个原因，在数组上不应该使用for/in循环，除非使用额外的检测方法来过滤不想要的属性。如：

```javascript
for(var i in a) {
	if (!a.hasOwnProperty(i)) continue;
	// 循环体
}

for(var i in a) {
	// 跳过不是非负整数的i
	if (String(Math.floor(Math.abs(Number(i)))) !== i) continue;
	// 循环体
}
```

ECMAScript规范允许for/in循环以不同的顺序遍历对象的属性。通常数组元素的遍历实现是升序的，但不能保证一定是这样的。特别地，如果数组同时拥有对象属性和数组元素时，返回的属性名很可能是按照创建的顺序而非数值的大小顺序。

ECMAScript 5定义了一些遍历数组元素的新方法，如：

```javascript
var data = [1, 2, 3, 4, 5];
var sumOfSquares = 0;
data.forEach(function(x) {
				sumOfSquares += x*x;
			});
sumOfSquares;			// 55
```

## 7.7 多维数组
Javascript不支持真正的多维数组，但可以用数组的数组来近似。访问数组的数组中的元素，只要简单地使用两次[]操作符即可。如：

```javascript
// 创建一个多维数组
var table = new Array(10);
for(var i = 0; i < table.length; i++)
    table[i] = new Array(10);
for(var row = 0; row < table.length; row++) {
    for(col = 0; col < table[row].length; col++) {
        table[row][col] = row*col;
    }
}

// 查询5*7
var product = table[5][7];    // 35
```

## 7.8 数组方法
ECMAScript 3在Array.prototype中定义了一些操作数组的函数，这些函数在任何数组中都是可以使用的。

### 7.8.1 join()
Array.join()方法将数组中所有元素都转化为字符串并连接在一起，返回最后生成的字符串。可以指定一个可选的字符串在生成的字符串中来分隔数组的各个元素。如果不指定分隔符，默认使用逗号。如：

```javascript
var a = [1, 2, 3];
a.join();				// "1,2,3"
a.join(" ");			// "1 2 3"
var b = new Array(10);
b.join('-');			// "---------"
```

Array.join()方法是String.split()方法的逆向操作，后者是将字符串分割成若干块来创建一个数组。

### 7.8.2 reverse()
Array.reverse()方法将数组中的元素颠倒顺序，返回逆序的数组。它采取了替换；它不通过重新排列的元素创建新的数组，而是在原先的数组中重新排列它们。如：

```javascript
var a = [1, 2, 3];
a.reverse().join();		// "3,2,1"
```

### 7.8.3 sort()
Array.sort()方法将数组中的元素排序并返回排序后的数组。当不带参数调用sort()时，数组元素以字母表顺序排序，如：

```javascript
var a = new Array("banana", "cherry", "apple");
a.sort();
var s = a.join(", ");	// ["apple", "banana", "cherry"]
```

如果数组包含undefined元素，它们会被排到数组的尾部。

为了按照其他方式而非字母表顺序进行数组排序，必须给sort()方法传递一个比较函数。该函数决定了它的两个参数在序的数组中的先后顺序。假设第一个参数应该在前，比较函数应该返回一个小于0的数值。反之，假设第一个参数应该在后，函数应该返回一个大于0的数值。并且，假设两个值相等，函数返回0。如想按照数值大小进行排序：

```javascript
var a = [33, 4, 1111, 222];
a.sort();						// [1111, 222, 33, 4]
a.sort(function(a, b) {
			return a - b;
		});						// [4, 33, 222, 1111]
a.sort(function(a, b) { 
			return b - a; 
		 });					// [1111, 222, 33, 4]
```

另一个例子，将字符中数组不区分大小写按照字母表顺序排序：

```javascript
var a = ['ant', 'Bug', 'cat', 'Dog'];
a.sort();						// ["Bug", "Dog", "ant", "cat"]
a.sort(function(s, t) {
			var a = s.toLowerCase();
			var b = t.toLowerCase();
			if (a < b) return -1;
			if (a > b) return 1;
			return 0;
		});						// ["ant", "Bug", "cat", "Dog"]
```

### 7.8.4 concat()
Array.concat()方法创建并返回一个新数组，它的元素包括调用concat()的原始数组的元素和concat()的每个参数。如果参数中的任何一个自身是数组，则连接的是数组的元素，而数组本身。但要注意，concat()不会递归扁平化数组的数组。concat()也不会修改调用的数组。如：

```javascript
var a = [1, 2, 3];
a.concat(4, 5);				// [1, 2, 3, 4, 5]
a.concat([4, 5]);			// [1, 2, 3, 4, 5]
a.concat([4, 5], [6, 7]);	// [1, 2, 3, 4, 5, 6, 7]
a.concat(4, [5, 6, 7]);		// [1, 2, 3, 4, 5, 6, 7]
```

### 7.8.5 slice()
Array.slice()方法返回指定数组的一个片段或子数组。它的两个参数分别指定了片段的开始和结束的位置。返回的数组包含第一个参数指定的位置和所有到但不包含第二个参数指定的位置之间的所有数组元素。如果只指定一个参数，返回的数组将包含从开始位置到数组结尾的所有元素。如果参数出现负数，它表示相对于数组中最后一个元素的位置。如，-1指定了最后一个元素。slice()不会修改调用的数组。如：

```javascript
var a = [1, 2, 3, 4, 5];
a.slice(0, 3);				// [1, 2, 3]
a.slice(3);					// [4, 5]
a.slice(1, -1);				// [2, 3, 4]
a.slice(-3, -2);			// [3]
```

### 7.8.6 splice()
Array.splice()方法是在数组中插入或删除元素的通用方法，它不同于slice()和concat()，splice()会修改调用的数组。

splice()能够从数组中删除元素、插入元素到数组中或者同时完成这两种操作。在插入或删除点之后的数组元素会根据需要增加或减小它们的索引，因此数组的其他部分仍保持连续的。第一个参数指定了插入或删除的起始位置。第二个参数指定了应该从数组中删除的元素的个数。如果省略第二个参数，从起点开始到数组结尾的所有元素都将被删除。splice()返回一个由删除元素组成的数组，或者如果没有删除元素就返回一个空数组。如：

```javascript
var a = [1, 2, 3, 4, 5, 6, 7, 8];
a.splice(4);			// [5, 6, 7, 8]; a = [1, 2, 3, 4]
a.splice(1, 2);			// [2, 3]; a = [1, 4]
a.splice(1, 1);			// [4]; a = [1]
```

splice()的前两个参数指定了需要删除的数组元素。紧随其后的任意个数的参数指定了需要插入到数组中的元素，从第一个参数指定的位置开始插入，如：

```javascript
var a = [1, 2, 3, 4, 5];
a.splice(2, 0, 'a', 'b');		// []; a = [1, 2, "a", "b", 3, 4, 5]
a.splice(2, 2, [1, 2], 3);		// ["a", "b"]; a = [1, 2, [1, 2], 3, 3, 4, 5]
```

### 7.8.7 push() and pop()
push()和pop()方法允许将数组当作栈来使用。push()方法在数组的尾部添加一个或多个元素，并返回数组新的长度。pop()方法则相反，它删除数组的最后一个元素，减小数组长度并返回它删除的值。注意，两个方法都修改并替换原始数组而非生成一个修改版的新数组。如：

```javascript
var stack = [];
stack.push(1, 2);			// 2 stack: [1, 2]
stack.pop();				// 2 stack: [1]
stack.push(3);				// 2 stack: [1, 3]
stack.pop();				// 3 stack: [1]
stack.push([4, 5]);			// 2 stack: [1, [4, 5]]
stack.pop();				// [4, 5] stack: [1]
stack.pop();				// 1 stack: []
```

### 7.8.8 unshift() and shift()
unshift()和shift()方法的行为非常类似于push()和pop()，不一样的是前者在数组的头部而非进行元素的插入和删除操作。unshift()在数组头部添加一个或多个元素，并将已存在的元素移动到更高索引的位置来获得足够的空间，最后返回数组新的长度。shift()删除数组的第一个元素并将其返回，然后把所有随后的元素下移一个位置来填补数组头部的空缺。如:

```javascript
var a = [];
a.unshift(1);			// 1 a:[1]
a.unshift(22);			// 2 a:[22, 1]
a.shift();				// 22 a:[1]
a.unshift(3, [4, 5]);	// 3 a:[3, [4, 5], 1]
a.shift();				// 3 a:[[4, 5], 1]
a.shift();				// [4, 5] a:[1]
a.shift();				// 1 a:[]
```

当使用多个参数调用unshift()时，参数是一次性插入的，而非一次一个地插入。

### 7.8.9 toString() and toLocaleString()
数组和其他Javascript对象一样拥有toString()方法。针对数组，该方法将其每个元素转化为字符串并输出用逗号分隔的字符串列表。如：

```javascript
[1, 2, 3].toString();			// "1,2,3"
["a", "b", "c"].toString();		// "a,b,c"
[1, [2, 'c']].toString();		// "1,2,c"
```

这里与不使用任何参数调用join()方法返回的字符串是一样的。

toLocaleString()是toString()方法的本地化版本。它调用元素的toLocaleString()方法将每个元素转化为字符串，并且使用本地化分隔符将这些字符串连接起来生成最终的字符串。

## 7.9 ECMAScript 5中的数组方法
ECMAScript 5定义了9个新的数组方法来遍历、映射、过滤、检测、简化和搜索数组。大多数方法的第一个参数接收一个函数，并且对数组的每个元素(或一些元素)调用一次该函数。如果是稀疏数组，对不存在的元素不调用该函数。在多数情况下，调用提供的函数使用三个参数：数组元素、元素的索引和数组本身。通常，只需要第一个参数值，可以忽略后两个参数。ECMAScript 5数组方法的第二个参数是可选的，如果使用第二个参数，则调用的函数被看做是第二个参数的方法。被调用函数的返回值非常重要，但是不同的方法处理返回值的方式也不一样。ECMAScript 5中的数组方法都不会修改它们调用的原始数组。但传递给这些方法的函数是可以修改这些数组的。

### 7.9.1 forEach()
forEach()方法从头至尾遍历数组，为每个元素调用指定的函数。传递的函数作为forEach()的第一个参数。然后forEach()使用三个参数调用函数：数组元素、元素的索引和数组本身。如：

```javascript
var data = [1, 2, 3, 4, 5];
var sum = 0;
data.forEach(function(value) { sum += value; });
console.log(sum);									// 15
data.forEach(function(v, i, a) { a[i] += 1; });
console.log(data);									// [2, 3, 4, 5, 6]
```

forEach()方法无法在所有元素都传递给调用的函数之前终止遍历。也就是说，没有像for循环中使用的相应的break语句。如果要提前终止，必须把forEach()方法放在一个try块中，并能抛出一个异常。如：

```javascript
function foreach(a, f, t) {
	try { a.forEach(f, t); }
	catch(e) {
		if (e === foreach.break) return;
		else throw e;
	}
}
foreach.break = new Error("StopIteration");
```

### 7.9.2 map()
map()方法将调用的数组的每个元素传递给指定的函数，并返回一个数组，它包含该函数的返回值。如：

```javascript
a = [1, 2, 3];
b = a.map(function(x) { return x*x; });		// [1, 4, 9]
```

传递给map()的函数的调用方式和传递给forEach()的函数的调用方式一样。但传递给map()的函数应该有返回值。注意，map()返回的是新数组：它不修改调用的数组。如果是稀疏数组，返回的也是相同方式的稀疏数组：它具有相同的长度，相同的缺失元素。

### 7.9.3 filter()
filter()方法返回的数组元素是调用的数组的一个子集。传递的函数是用来逻辑判断的：返回true或false。如果返回值为true或能转化为true的值，那么传递给判定函数的元素就是这个子集的成员，它将被添加到一个作为返回值的数组中。如：

```javascript
a = [5, 4, 3, 2, 1];
smallvalues = a.filter(function(x) { return x < 3; });			// [2, 1]
everyother = a.filter(function(x, i) { return i % 2 == 0; });	// [5, 3, 1]
```

filter会跳过稀疏数组中缺少的元素，它的返回数组总是稠密的。将稀疏数组转换为非稀疏数组可以使用如下方法：

```javascript
var dense = sparse.filter(function() { return true; });
```

压缩空缺并删除undefined和null元素，可以这样使用：

```javascript
a = a.filter(function(x) { return x !== undefined && x !== null; });
```

### 7.9.4 every() and some()
every()和some()方法是数组的逻辑判定：它们对数组元素应用指定的函数进行判定，返回true或false。

every()方法：当且仅当数组中的所有元素调用判定函数都返回true时，它才返回true。some()方法：当数组中至少有一个元素调用函数返回true，它就返回true。如：

```javascript
a = [1, 2, 3, 4, 5];
a.every(function(x) { return x < 10; });		// true
a.every(function(x) { return x % 2 === 0; });	// false
a.some(function(x) { return x % 2 === 0; });	// true
a.some(isNaN);									// false
```

一旦every()和some()方法确认返回值它们就是停止遍历数组元素。some()在判定函数第一次返回true后就返回true，如果判定函数一直返回false，它将会遍历整个数组。every()在判定函数第一次返回false后就返回false，如果判定函数一直返回true，它将会遍历整个数组。对空数组，every()返回true，some()返回false。

### 7.9.5 reduce() and reduceRight()
reduce()和reduceRigth()方法使用指定的函数将数组元素进行组合，生成单个值。如：

```javascript
var a = [1, 2, 3, 4, 5];
var sum = a.reduce(function(x, y) { return x + y; }, 0);		// sum:15
var product = a.reduce(function(x, y) { return x*y; }, 1);		// product:120
var max = a.reduce(function(x, y) { return x > y ? x : y; });	// max:5
```

reduce()方法需要两个参数。第一个参数被调用的函数，任务就是将两个值组合成一个值，并返回化简后的值。第二个参数为可选参数，是传递给被调用函数的初始值。被调用函数的第一个参数是目前为止的结果。第一次调用时，使用reduce()的第二个参数，如果没有提供第二个参数，则使用数组的第一个值。被调用函数的第二个参数是使用数组中当前值。

在空数组上，不指定第二个参数调用reduce()将导致类型错误异常。如果数组只有一个元素，不指定第二个参数调用reduce()或者空数组上指定第二个参数调用reduce()，将直接返回数组元素或者默认值。

reduceRight()的工作原理与reduce()方法一样，只是它从数组的右侧开始处理。

### 7.9.6 indexOf() and lastIndexOf()
indexOf()和lastIndexOf()搜索整个数组中具有给定值的元素，返回找到的第一个元素的索引或者如果没有找到就返回-1。index()从头至尾搜索，而lastIndexOf()则反向搜索：

```javascript
a = [0, 1, 2, 1, 0];
a.indexOf(1);			// 1
a.lastIndexOf(1);		// 3
a.indexOf(3);			// -1
```	

indexOf()和lastIndexOf()方法不接收一个函数作为其参数。第一个参数是需要搜索的值，第二个参数是可选的：它指定数组中开始搜索的一个索引。如果省略，indexOf()从头开始搜索，lastIndexOf()从末尾开始搜索。可以使用第二个参数将所有匹配元素的索引返回，如：

```javascript
function findall(a, x) {
	var results = [], len = a.length, pos = 0;
	while(pos < len) {
		pos = a.indexOf(x, pos);
		if (pos === -1) break;
		results.push(pos);
		pos ++;
	}
	return results;
}
```

字符串也有indexOf()和lastIndexOf()方法，它们和数组方法的功能类似。

## 7.10 数组类型
数组是具有特殊行为的对象。给定一个对象，判定它是否为数组通常非常有用。在ECMAScript 5中，可以使用Array.isArray()函数来做这件事情：

```javascript
Array.isArray([]);	// true
Array.isArray({});	// false
```

但是，在ECMAScript 5之前，要区分数组和非数组对象却很困难。typeof操作符也不行：对数组返回对象，除函数外的所有对象都如此。instanceof操作符也只能应用于简单的形式，如：

```javascript
[] instanceof Array;		// true
([]) instanceof Array;		// false
```

解决方案是检查对象的类属性，对数组而言该属性的值是"Array"，因此在ECMAScript 3中isArray()函数可以这样写：

```javascript
var isArray = Array.isArray || function(o) {
	return typeof o === "object" &&
		Object.prototype.toString.call(o) === "[object Array]";
};
```

## 7.11 类数组对象
Javascript数组的一些特性是其他对象没有的：
* 当有新的元素添加到列表中时，自动更新length属性。
* 设置length为一个较小的值将截断数组。
* 从Array.prototype中继承一些有用的方法。
* 某类属性为Array。

这些特性让Javascript数组和常规的对象有明显的区别。但是它们不是定义数组的本质特性。把拥有一个数值length属性和对应非负整数属性的对象看做一种类型的数组。以下代码为一个常规对象增加了一些属性使其变成类数组对象，然后遍历生成的伪数组的元素:

```javascript
var a = [];
var i = 0;
while(i < 10) {
	a[i] = i * i;
	i++;
}
a.length = i;

var total = 0;
for(var j = 0; j < a.length; j++)
	total += a[j];
```

在客户端Javascript中，一些DOM方法(document.getElementsByTagName())也返回类数组对象。下面的方法可以检测类数组对象。

```javascript
/* 
 * 判断o是否是一个类数组对象
 * 字符串和函数有length属性，可用typeof检测排除
 * DOM文本节点也有length属性，需要用o.nodeType != 3将其排除
 */
function isArrayLike(o) {
	if (o &&								// o非null、undefined等
		typeof o === "object" &&			// 非对象
		isFinite(o.length) &&				// o.length是有限数值
		o.length >= 0 &&					// o.length是非负数
		o.length === Math.floor(o.length) &&// o.length是整数
		o.length < 4294967296)				// o.length < 2^32
		return true;
	else
		return false;
}
```

## 7.12 作为数组的字符串
在ECMAScript 5中，字符串的行为类似于只读的数组。除了用charAt()方法来访问单个的字符以外，可可以使用方括号：

```javascript
var s = 'test';
s.charAt(0);			// "t"
s[1];					// "e"
```

针对字符串的typeof操作符仍然返回"string"，但是如果给Array.isArray()传递字符串，这将返回false。

可索引的字符串的最大的好处就是简单，用方括号代替了charAt()调用，这样更加简洁、可读并且可能更高效。不公如此，字符串的行为类似于数组的事实使得通用的数组方法可以应用到字符串上，如：

```javascript
s = "Javascript";
Array.prototype.join.call(s, " ");			// "J a v a s c r i p t"
Array.prototype.filter.call(s,
	function(x) {
		return x.match(/[^aeiou]/);
	}).join("");							// "Jvscrpt"
```

字符串是不可变值，故当把它们作为数组看待时，它们是只读的。如push()、sort()、reverse()和splice()等数组方法会修改数组，它们在字符串上是无效的。这将导致出错，且没有提示。

Author website: [furzoom](http://furzoom.com/about-us/ "Furzoom")
