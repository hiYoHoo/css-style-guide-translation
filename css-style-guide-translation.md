# Dropbox (S)CSS Style Guide

##

> "Every line of code should appear to be written by a single person, no matter the number of contribitors."——@mdo

> “不管有多少人参与编写代码，也要让每一行代码看起来都像是一个人写出来的。”——@mdo

## Gereral

### Don'ts

- 避免在css选择器中使用HTML标签
  - 例如使用`.o-modal{}`要比`div.o-modal{}`要好一些
  - 使用类选择器而不是HTML标签（也有一些例外比如在CSS resets中）
- 不要使用id选择器
  - `#header`是过于具体的表示，而`.header`也已经很难被覆盖了。
  - 在[这里](http://csswizardry.com/2011/09/when-using-ids-can-be-a-pain-in-the-class/)了解更多CSS中有关ID的理解
- 不要嵌套3层以上结构
  - 嵌套选择器会增加唯一性，意味着要想覆盖其中的CSS需要更为特殊的选择器，这会成为一个很大的维护问题
- 除了伪选择器和状态选择器，避免在任何时候使用嵌套结构
  - 例如，嵌套`:hover`，`:focus`，`::before`等等，是可以的，但是在选择器中嵌套选择器应该尽量避免
- 不要使用`!important`
  - 永远不要
  - 如果一定要用，添加注释，并在借助`!important`之前使用其他能够做优先处理的方法
  - `!important`极大的增加了的CSS规则的权重，确保它在以后的修改中很难被覆盖。要想修改只能通过另一个`!important`去覆盖。
- 不要使用`margin-top`
  - 垂直方向上的外边距会[塌陷](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)。使用`padding-top`或者上一个元素的`margin-bottom`来替代
- 避免使用缩写属性（除非你有必要使用）
  - 人们倾向于使用缩写属性，例如使用`background: #fff`而不是`background-color: #fff`，但是这样做会重写缩写属性中包含的其他值。（这上面的例子中，`background-image`和其他关联的属性都会被设置为“none”）
  - 这一点适用于所有具有缩写的属性：border，margin，padding，font等等

### Spacing

- 使用4个空格作为代码缩进
- 在属性中`:`的后面使用空格
  - 例如使用`color: red;`而不是`color:red;`
- 在规则中`{`之前使用空格
  - 例如使用`.o-modal {`而不是`.o-modal{`
- 一条规则写一行
- 在`}`结束的规则后面空出一行
- 当有一组选择器并列时，保持每一个选择器在单独的一行
- 将关闭括号`}`放在新的一行
- 在.scss文件的末尾添加新的一行
- 删减多余的空格

### Formatting

- 所有的选择器使用小写字母，通过连字符分隔单词（“脊式命名法”），例如`.my-class-name`
- 总是使用Sass的双斜线`//`来注释，也包括块注释
- 避免为零值指定单位，例如，使用`margin: 0;`而不是`margin: 0px;`
- 在每条属性名值对后添加分号
- 不要省略小数的前导零，例如使用`opacity: 0.4;`而不是`opacity: .4;`
- 在子选择器的前后加上空格，使用`div > span`而不是`div>span`

---

## Sass Specifics

### Internal order of a .scss file

1. Imports
2. Variables
3. Base Styles
4. Experiment Styles

例如:

	//----------------------
	// Modal
	//----------------------
	@import "../constants";
	@import "../helpers";

	$DBmodal-namespace: "c-modal" !default;
	$DBmodal-padding: 32px;

	$DBmodal-background: #fff !default;
	$DBmodal-background-alt: color(gray, x-light) !default;

	.o-modal { ... }

	// Many lines later...

	// EXPERIMENT: experiment-rule-name
	.o-modal--experiment { ... }
	// END EXPERIMENT: experiment-rule-name

### Variables

- 在文件顶部imports之后定义所有的变量
- 使用文件名命名局部变量（SASS没有文件作用域）
  - 例如`business_contact.scss`→`$business_contact_font_size: 14px;`
- 局部变量应该是这样的：`$snake_lowercase`
- 全局变量应该是这样的：`$SNAKE_ALL_CAPS`

### Color

- 使用通过color函数定义的颜色常量
- 使用小写的十六进制表示的颜色值 `#ffffff`
- 限制alpha的值最多为2个小数位

例如：

	//Bad
	.c-link {
  		color: #007ee5;
  		border-color: #FFF;
  		background-color: rgba(#FFF, .0625);
	}

	// Good
	.c-link {
  		color: color(blue);
  		border-color: #ffffff;
  		background-color: rgba(#ffffff, 0.1);
	}

### Experiments

使用注释包裹试验样式

	// EXPERIMENT: experiment-rule-name
	.stuff { ... }
	// END EXPERIMENT: experiment-rule-name

---

## Rule Ordering

属性和嵌套的声明应该按照下面的顺序出现，每组之间空一行：

1. 所有的 `@` 规则
2. 布局和盒模型属性
  - margin,padding,box-sizing,overflow,position,display,width/height,等等
3. 排版属性
  - font-,line-height,letter-spacing,text-,etc.
4. 格式属性
  - color,background-,animation,border,etc.
5. 界面属性
  - appearance,cursor,user-select,pointer-events,etc.
6. 伪类
  - ::after,::before,::selection,etc.
7. 伪选择器
  - :hover,:focus,:active,etc.
8. 修饰类
9. 嵌套元素

下面是一个全面的例子：

	.c-btn {
	    @extend %link--plain;
	
	    display: inline-block;
	    padding: 6px 12px;
	
	    text-align: center;
	    font-weight: 600;
	
	    background-color: color(blue);
	    border-radius: 3px;
	    color: white;
	
	    &::before {
	        content: '';
	    }
	
	    &:focus, &:hover {
	        box-shadow: 0 0 0 1px color(blue, .3);
	    }
	
	    &--big {
	        padding: 12px 24px;
	    }
	
	    > .c-icon {
	        margin-right: 6px;
	    }
	}

---

## Nesting

- 一般的经验，避免超过3层的嵌套
- 使用嵌套便于扩展父选择器，例如：

		.block {
    		padding: 24px;

    		&--mini {
        		padding: 12px;
    		}
		}

通过巧妙地命名类名（参考BEM）可以避免嵌套也能避免单独的标签选择器。

---

## BEM

Block：唯一的，有意义，合乎逻辑的样式命名。避免过度的缩写。

- Good：`.alert-box`或`.recents-intro`或`.button`
- Bad：`.feature`或`.content`或`.btn`

Element：应用于块的子元素的样式。元素本身也可以是块。类名由块名，两个下划线和元素名连接。例如：

- `.alert-box__close`
- `.expanding-section__section`

Modifier：通过修饰样式重写或扩展块或者元素的样式。类名由块名（或者元素名），两个连字符和修饰名连接，例如：

- `alert-box--success`
- `.expanding-section--expanded`

## BEM Best practices

不要在基础块中扩展修饰。

- Good：`<div class="my-block my-block--modifier">`
- Bad：`<div class="my-block--modifier">`

不要再元素中创建元素。如果有必要，可以考虑将元素放在块中。

- Bad：`.alert-box__close__button`

明智的选择你的修饰名。下面两个规则有明显不同的含义：

	.block--modifier .block__element { color: red; }
	.block__element--modifier { color: red; }

## Selector Naming

- 试着在你的类选择器中使用[BEM-based](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
  - 使用修饰类时，确保上一层级命名不是修饰类。
- 像下面这样使用Sass的嵌套来设计BEM选择器：

		.block {
		    &--modifier { // compiles to .block--modifier
		        text-align: center;
		    }
		
		    &__element { // compiles to .block__element
		        color: red;
		
		        &--modifier { // compiles to .block__element--modifier
		            color: blue;
		        }
		    }
		}

## Namespaced Classes

有一些预设的命名空间来避免一般的全局可获得的抽象类。

- `.o-`作为CSS对象。对象是常见的设计模式（就像Flag object）。修饰这些类会引起不好的连锁反应。
- `.c-`作为CSS组件。组件是为用户界面设计的——像按钮，表单，情景和广告之类的。
- `.u-`作为帮助和工具。工具往往具有单一的作用和较高的优先权。像一些浮动元素和裁剪边距等等。
- `.is-,.has-`作为状态，是一个[SMACSS](https://smacss.com/book/type-state)。适用于临时的，可选的，生命周期短的状态和样式。
- `._`用于hacks。带有hack的命名空间应该被用于你需要强制使用`!important`或需要增强优先级别的的地方，是临时的，不被约束的。
- `.t-`用于主题。带有特殊样式或者会覆盖所有对象或组件的页面应该使用主题类。

---

## Separation of Concerns (One Thing Well™)

你总是会用一些常见的代码——padding,font-sizes,layout patterns——抽离出他们重复使用，命名空间的类会被元素引用，它们具有单一的功能。这样做可以避免重写和重复的规则，有助于组件的分离。

	// Bad code
	// HTML:
	// <div class="modal compact">...</div>
	.modal {
	    padding: 32px;
	    background-color: color(gray, x-light);
	
	    &.compact {
	        padding: 24px;
	    }
	}
	
	// Good code
	// HTML:
	// <div class="c-modal u-l-island">...</div>
	// <div class="c-modal u-l-isle">...</div>
	
	// components/_modal.scss
	.c-modal {
	    background-color: color(gray, x-light);
	}
	
	// helpers/_layout.scss
	.u-l-island {
	    padding: 32px;
	}
	
	.u-l-isle {
	    padding: 24px;
	}

---

## Media Queries

媒体查询应该被包含在CSS选择器中作为一个SMACSS

	.selector {
	      float: left;
	
	      @media only screen and (max-width: 767px) {
	        float: none;
	      }
	}

为频繁使用的断点创建变量

	$SCREEN_SM_MAX: "max-width: 767px";
	
	.selector {
	      float: left;
	
	      @media only screen and ($SCREEN_SM_MAX) {
	        float: none;
	      }
	}