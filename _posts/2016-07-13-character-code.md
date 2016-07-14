---
title: 字符编码
author: k
layout: post
permalink: /character-code/
post_views_count:
  - 0
categories:
  - 字符编码
tags:
  - 字符编码
---

# 一：历史背景

## 1.为什么一个字节等于8个比特？

　　其实早期也出现过1byte=4bit 1byte=6bit 1byte=7bit，后来伴随着SYSTEM/360的流行，最终确定下来1byte=8bit

## 2.字符

　　一个信息单元。简单来说，一个汉字，一个英文字母，或者其他语言的字母都是一个字符。

## 3.字符集与字符编码的发展

　　计算机起源于美国，对英语来说，用7bit就可以囊括所有的常用的字符了，2^7=128，最高位第8bit为0，这就是ASCII码。后来其他一些欧洲国家也开始使用计算机了，那么现有的字符编码就不够用了，于是把最高位也用起来，2^8=256，也就是说把128-255也用起来，这就是扩展ASCII码，再到后来，全世界越来越多的国家开始使用计算机，256完全不够用了，于是各个国家自己想办法，采用不同的编码方法，就比如中国，就出现了很多种不同的编码方法，GB2312、BIG5、GB18030等，但是这样有一个问题，那就是国际化，汉字编码的东东拿到外国去就解码不了了，就是乱码，于是为了实现大一统，Unicode出现了，它使用4字节的数字来表达每个字母、符号，或者表意文字。

# 二：基础知识

## 1.编码&解码

　　计算机中储存的信息都是用二进制数表示的；而我们在屏幕上看到的英文、汉字等字符是二进制数转换之后的结果。通俗的说，按照何种规则将字符存储在计算机中，如'a'用什么表示，称为"编码"；反之，将存储在计算机中的二进制数解析显示出来，称为"解码"，如同密码学中的加密和解密。在解码过程中，如果使用了错误的解码规则，则导致'a'解析成'b'或者乱码。

## 2.字符集（Charset）

　　是一个系统支持的所有抽象字符的集合。字符是各种文字和符号的总称，包括各国家文字、标点符号、图形符号、数字等。

## 3.字符编码（Character Encoding）

　　是一套法则，使用该法则能够对自然语言的字符的一个集合（如字母表或音节表），与其他东西的一个集合（如号码或电脉冲）进行配对。即在符号集合与数字系统之间建立对应关系，它是信息处理的一项基本技术。通常人们用符号集合（一般情况下就是文字）来表达信息。而以计算机为基础的信息处理系统则是利用元件（硬件）不同状态的组合来存储和处理信息的。元件不同状态的组合能代表数字系统的数字，因此字符编码就是将符号转换为计算机可以接受的数字系统的数，称为数字代码。

## 4.常用字符集和字符编码

　　常见字符集名称：ASCII字符集、GB2312字符集、BIG5字符集、GB18030字符集、Unicode字符集等。计算机要准确的处理各种字符集文字，需要进行字符编码，以便计算机能够识别和存储各种文字。

# 三 常见字符编码

## 1.ASCII

　　ASCII（American Standard Code for Information Interchange，美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统。它主要用于显示现代英语，而其扩展版本EASCII则可以勉强显示其他西欧语言。它是现今最通用的单字节编码系统（但是有被Unicode追上的迹象），并等同于国际标准ISO/IEC 646。

　　ASCII字符集：主要包括控制字符（回车键、退格、换行键等）；可显示字符（英文大小写字符、阿拉伯数字和西文符号）。

　　ASCII编码：将ASCII字符集转换为计算机可以接受的数字系统的数的规则。使用7位（bits）表示一个字符，共128字符；但是7位编码的字符集只能支持128个字符，为了表示更多的欧洲常用字符对ASCII进行了扩展，ASCII扩展字符集使用8位（bits）表示一个字符，共256字符。

　　ASCII的最大缺点是只能显示26个基本拉丁字母、阿拉伯数目字和英式标点符号，因此只能用于显示现代美国英语（而且在处理英语当中的外来词如na?ve、café、élite等等时，所有重音符号都不得不去掉，即使这样做会违反拼写规则）。而EASCII虽然解决了部份西欧语言的显示问题，但对更多其他语言依然无能为力。因此现在的苹果电脑已经抛弃ASCII而转用Unicode。

## 2.GBXXXX字符集&编码

　　计算机发明之处及后面很长一段时间，只用应用于美国及西方一些发达国家，ASCII能够很好满足用户的需求。但是当天朝也有了计算机之后，为了显示中文，必须设计一套编码规则用于将汉字转换为计算机可以接受的数字系统的数。

### 1.GB2312

　　天朝专家把那些127号之后的奇异符号们（即EASCII）取消掉，规定：一个小于127的字符的意义与原来相同，但两个大于127的字符连在一起时，就表示一个汉字，前面的一个字节（他称之为高字节）从0xA1用到 0xF7，后面一个字节（低字节）从0xA1到0xFE，这样我们就可以组合出大约7000多个简体汉字了。在这些编码里，还把数学符号、罗马希腊的 字母、日文的假名们都编进去了，连在ASCII里本来就有的数字、标点、字母都统统重新编了两个字节长的编码，这就是常说的"全角"字符，而原来在127号以下的那些就叫"半角"字符了。

　　上述编码规则就是GB2312。GB2312或GB2312-80是中国国家标准简体中文字符集，全称《信息交换用汉字编码字符集·基本集》，又称GB0，由中国国家标准总局发布，1981年5月1日实施。GB2312编码通行于中国大陆；新加坡等地也采用此编码。中国大陆几乎所有的中文系统和国际化的软件都支持GB2312。GB2312的出现，基本满足了汉字的计算机处理需要，它所收录的汉字已经覆盖中国大陆99.75%的使用频率。对于人名、古汉语等方面出现的罕用字，GB2312不能处理，这导致了后来GBK及GB 18030汉字字符集的出现。

　　由于GB 2312-80只收录6763个汉字，有不少汉字，如部分在GB 2312-80推出以后才简化的汉字（如"啰"），部分人名用字（如中国前总理朱镕基的"镕"字），台湾及香港使用的繁体字，日语及朝鲜语汉字等，并未有收录在内。于是厂商微软利用GB 2312-80未使用的编码空间，收录GB 13000.1-93全部字符制定了GBK编码。根据微软资料，GBK是对GB2312-80的扩展，也就是CP936字码表 (Code Page 936)的扩展（之前CP936和GB 2312-80一模一样），最早实现于Windows 95简体中文版。虽然GBK收录GB 13000.1-93的全部字符，但编码方式并不相同。GBK自身并非国家标准，只是曾由国家技术监督局标准化司、电子工业部科技与质量监督司公布为"技术规范指导性文件"。原始GB13000一直未被业界采用，后续国家标准GB18030技术上兼容GBK而非GB13000。

### 2.GB18030

　　GB 18030，全称：国家标准GB 18030-2005《信息技术 中文编码字符集》，是中华人民共和国现时最新的内码字集，是GB 18030-2000《信息技术 信息交换用汉字编码字符集 基本集的扩充》的修订版。与GB 2312-1980完全兼容，与GBK基本兼容，支持GB 13000及Unicode的全部统一汉字，共收录汉字70244个。GB 18030主要有以下特点：

　　•与UTF-8相同，采用多字节编码，每个字可以由1个、2个或4个字节组成。

　　•编码空间庞大，最多可定义161万个字符。

　　•支持中国国内少数民族的文字，不需要动用造字区。

　　•汉字收录范围包含繁体汉字以及日韩汉字

　　本规格的初版使中华人民共和国信息产业部电子工业标准化研究所起草，由国家质量技术监督局于2000年3月17日发布。现行版本为国家质量监督检验总局和中国国家标准化管理委员会于2005年11月8日发布，2006年5月1日实施。此规格为在中国境内所有软件产品支持的强制规格。

### 3.Big5

　　Big5，又称为大五码或五大码，是使用繁体中文（正体中文）社区中最常用的电脑汉字字符集标准，共收录13,060个汉字。中文码分为内码及交换码两类，Big5属中文内码，知名的中文交换码有CCCII、CNS11643。Big5虽普及于台湾、香港与澳门等繁体中文通行区，但长期以来并非当地的国家标准，而只是业界标准。倚天中文系统、Windows等主要系统的字符集都是以Big5为基准，但厂商又各自增加不同的造字与造字区，派生成多种不同版本。2003年，Big5被收录到CNS11643中文标准交换码的附录当中，取得了较正式的地位。这个最新版本被称为Big5-2003。

　　Big5码是一套双字节字符集，使用了双八码存储方法，以两个字节来安放一个字。第一个字节称为"高位字节"，第二个字节称为"低位字节"。"高位字节"使用了0x81-0xFE，"低位字节"使用了0x40-0x7E，及0xA1-0xFE。在Big5的分区中：

	0x8140-0xA0FE	保留给用户自定义字符（造字区）
	0xA140-0xA3BF	标点符号、希腊字母及特殊符号，包括在0xA259-0xA261，安放了九个计量用汉字：兙兛兞兝兡兣嗧瓩糎。
	0xA3C0-0xA3FE	保留。此区没有开放作造字区用。
	0xA440-0xC67E	常用汉字，先按笔划再按部首排序。
	0xC6A1-0xC8FE	保留给用户自定义字符（造字区）
	0xC940-0xF9D5	次常用汉字，亦是先按笔划再按部首排序。
	0xF9D6-0xFEFE	保留给用户自定义字符（造字区）

## 3.Unicode字符集&UTF编码

　　像天朝一样，当计算机传到世界各个国家时，为了适合当地语言和字符，设计和实现类似GB232/GBK/GB18030/BIG5的编码方案。这样各搞一套，在本地使用没有问题，一旦出现在网络中，由于不兼容，互相访问就出现了乱码现象。

　　为了解决这个问题，一个伟大的创想产生了——Unicode。Unicode编码系统为表达任意语言的任意字符而设计。它使用4字节的数字来表达每个字母、符号，或者表意文字(ideograph)。每个数字代表唯一的至少在某种语言中使用的符号。（并不是所有的数字都用上了，但是总数已经超过了65535，所以2个字节的数字是不够用的。）被几种语言共用的字符通常使用相同的数字来编码，除非存在一个在理的语源学(etymological)理由使不这样做。不考虑这种情况的话，每个字符对应一个数字，每个数字对应一个字符。即不存在二义性。不再需要记录"模式"了。U+0041总是代表'A'，即使这种语言没有'A'这个字符。

　　在计算机科学领域中，Unicode（统一码、万国码、单一码、标准万国码）是业界的一种标准，它可以使电脑得以体现世界上数十种文字的系统。Unicode 是基于通用字符集（Universal Character Set）的标准来发展，并且同时也以书本的形式[1]对外发表。Unicode 还不断在扩增， 每个新版本插入更多新的字符。直至目前为止的第六版，Unicode 就已经包含了超过十万个字符（在2005年，Unicode 的第十万个字符被采纳且认可成为标准之一）、一组可用以作为视觉参考的代码图表、一套编码方法与一组标准字符编码、一套包含了上标字、下标字等字符特性的枚举等。Unicode 组织（The Unicode Consortium）是由一个非营利性的机构所运作，并主导 Unicode 的后续发展，其目标在于：将既有的字符编码方案以Unicode 编码方案来加以取代，特别是既有的方案在多语环境下，皆仅有有限的空间以及不兼容的问题。

　　（可以这样理解：Unicode是字符集，UTF-32/ UTF-16/ UTF-8是三种字符编码方案。）


### 3.1.UCS & UNICODE

　　通用字符集（Universal Character Set，UCS）是由ISO制定的ISO 10646（或称ISO/IEC 10646）标准所定义的标准字符集。历史上存在两个独立的尝试创立单一字符集的组织，即国际标准化组织（ISO）和多语言软件制造商组成的统一码联盟。前者开发的 ISO/IEC 10646 项目，后者开发的统一码项目。因此最初制定了不同的标准。

　　1991年前后，两个项目的参与者都认识到，世界不需要两个不兼容的字符集。于是，它们开始合并双方的工作成果，并为创立一个单一编码表而协同工作。从Unicode 2.0开始，Unicode采用了与ISO 10646-1相同的字库和字码；ISO也承诺，ISO 10646将不会替超出U+10FFFF的UCS-4编码赋值，以使得两者保持一致。两个项目仍都存在，并独立地公布各自的标准。但统一码联盟和ISO/IEC JTC1/SC2都同意保持两者标准的码表兼容，并紧密地共同调整任何未来的扩展。在发布的时候，Unicode一般都会采用有关字码最常见的字型，但ISO 10646一般都尽可能采用Century字型。

### 3.2.UTF-32

　　上述使用4字节的数字来表达每个字母、符号，或者表意文字(ideograph)，每个数字代表唯一的至少在某种语言中使用的符号的编码方案，称为UTF-32。UTF-32又称UCS-4是一种将Unicode字符编码的协定，对每个字符都使用4字节。就空间而言，是非常没有效率的。

这种方法有其优点，最重要的一点就是可以在常数时间内定位字符串里的第N个字符，因为第N个字符从第4×Nth个字节开始。虽然每一个码位使用固定长定的字节看似方便，它并不如其它Unicode编码使用得广泛。

### 3.3.UTF-16

　　尽管有Unicode字符非常多，但是实际上大多数人不会用到超过前65535个以外的字符。因此，就有了另外一种Unicode编码方式，叫做UTF-16(因为16位 = 2字节)。UTF-16将0–65535范围内的字符编码成2个字节，如果真的需要表达那些很少使用的"星芒层(astral plane)"内超过这65535范围的Unicode字符，则需要使用一些诡异的技巧来实现。UTF-16编码最明显的优点是它在空间效率上比UTF-32高两倍，因为每个字符只需要2个字节来存储（除去65535范围以外的），而不是UTF-32中的4个字节。并且，如果我们假设某个字符串不包含任何星芒层中的字符，那么我们依然可以在常数时间内找到其中的第N个字符，直到它不成立为止这总是一个不错的推断。其编码方法是：

　　•如果字符编码U小于0x10000，也就是十进制的0到65535之内，则直接使用两字节表示；

　　•如果字符编码U大于0x10000，由于UNICODE编码范围最大为0x10FFFF，从0x10000到0x10FFFF之间 共有0xFFFFF个编码，也就是需要20个bit就可以标示这些编码。用U'表示从0-0xFFFFF之间的值，将其前 10 bit作为高位和16 bit的数值0xD800进行 逻辑or 操作，将后10 bit作为低位和0xDC00做 逻辑or 操作，这样组成的 4个byte就构成了U的编码。

　　对于UTF-32和UTF-16编码方式还有一些其他不明显的缺点。不同的计算机系统会以不同的顺序保存字节。这意味着字符U+4E2D在UTF-16编码方式下可能被保存为4E 2D或者2D 4E，这取决于该系统使用的是大尾端(big-endian)还是小尾端(little-endian)。（对于UTF-32编码方式，则有更多种可能的字节排列。）只要文档没有离开你的计算机，它还是安全的——同一台电脑上的不同程序使用相同的字节顺序(byte order)。但是当我们需要在系统之间传输这个文档的时候，也许在万维网中，我们就需要一种方法来指示当前我们的字节是怎样存储的。不然的话，接收文档的计算机就无法知道这两个字节4E 2D表达的到底是U+4E2D还是U+2D4E。

　　为了解决这个问题，多字节的Unicode编码方式定义了一个"字节顺序标记(Byte Order Mark)"，它是一个特殊的非打印字符，你可以把它包含在文档的开头来指示你所使用的字节顺序。对于UTF-16，字节顺序标记是U+FEFF。如果收到一个以字节FF FE开头的UTF-16编码的文档，你就能确定它的字节顺序是单向的(one way)的了；如果它以FE FF开头，则可以确定字节顺序反向了。

### 3.4.UTF-8

　　UTF-8（8-bit Unicode Transformation Format）是一种针对Unicode的可变长度字符编码（定长码），也是一种前缀码。它可以用来表示Unicode标准中的任何字符，且其编码中的第一个字节仍与ASCII兼容，这使得原来处理ASCII字符的软件无须或只须做少部份修改，即可继续使用。因此，它逐渐成为电子邮件、网页及其他存储或传送文字的应用中，优先采用的编码。互联网工程工作小组（IETF）要求所有互联网协议都必须支持UTF-8编码。

　　UTF-8使用一至四个字节为每个字符编码：

　　•128个US-ASCII字符只需一个字节编码（Unicode范围由U+0000至U+007F）。

　　•带有附加符号的拉丁文、希腊文、西里尔字母、亚美尼亚语、希伯来文、阿拉伯文、叙利亚文及它拿字母则需要二个字节编码（Unicode范围由U+0080至U+07FF）。

　　•其他基本多文种平面（BMP）中的字符（这包含了大部分常用字）使用三个字节编码。

　　•其他极少使用的Unicode辅助平面的字符使用四字节编码。

　　UTF-8最大的一个特点，就是它是一种变长的编码方式。它可以使用1~4个字节表示一个符号，根据不同的符号而变化字节长度。

　　UTF-8的编码规则很简单，只有二条：

　　1）对于单字节的符号，字节的第一位设为0，后面7位为这个符号的unicode码。因此对于英语字母，UTF-8编码和ASCII码是相同的。

　　2）对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的unicode码。

　　下表总结了编码规则，字母x表示可用编码的位。

	Unicode符号范围 | UTF-8编码方式
	(十六进制) | （二进制）
	--------------------+---------------------------------------------
	0000 0000-0000 007F | 0xxxxxxx
	0000 0080-0000 07FF | 110xxxxx 10xxxxxx
	0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
	0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx

　　跟据上表，解读UTF-8编码非常简单。如果一个字节的第一位是0，则这个字节单独就是一个字符；如果第一位是1，则连续有多少个1，就表示当前字符占用多少个字节。

　　下面，还是以汉字"严"为例，演示如何实现UTF-8编码。

　　已知"严"的unicode是4E25（100111000100101），根据上表，可以发现4E25处在第三行的范围内（0000 0800-0000 FFFF），因此"严"的UTF-8编码需要三个字节，即格式是"1110xxxx 10xxxxxx 10xxxxxx"。然后，从"严"的最后一个二进制位开始，依次从后向前填入格式中的x，多出的位补0。这样就得到了，"严"的UTF-8编码是"11100100 10111000 10100101"，转换成十六进制就是E4B8A5。

　　下面，举一个实例。

　　打开"记事本"程序Notepad.exe，新建一个文本文件，内容就是一个"严"字，依次采用ANSI，Unicode，Unicode big endian 和 UTF-8编码方式保存。

　　然后，用文本编辑软件UltraEdit中的"十六进制功能"，观察该文件的内部编码方式。

	1）ANSI：文件的编码就是两个字节"D1 CF"，这正是"严"的GB2312编码，这也暗示GB2312是采用大头方式存储的。

	2）Unicode：编码是四个字节"FF FE 25 4E"，其中"FF FE"表明是小头方式存储，真正的编码是4E25。

	3）Unicode big endian：编码是四个字节"FE FF 4E 25"，其中"FE FF"表明是大头方式存储。

	4）UTF-8：编码是六个字节"EF BB BF E4 B8 A5"，前三个字节"EF BB BF"表示这是UTF-8编码，后三个"E4B8A5"就是"严"的具体编码，它的存储顺序与编码顺序是一致的。

　　在处理经常会用到的ASCII字符方面非常有效。在处理扩展的拉丁字符集方面也不比UTF-16差。对于中文字符来说，比UTF-32要好。同时，（在这一条上你得相信我，因为我不打算给你展示它的数学原理。）由位操作的天性使然，使用UTF-8不再存在字节顺序的问题了。一份以utf-8编码的文档在不同的计算机之间是一样的比特流。

　　总体来说，在Unicode字符串中不可能由码点数量决定显示它所需要的长度，或者显示字符串之后在文本缓冲区中光标应该放置的位置；组合字符、变宽字体、不可打印字符和从右至左的文字都是其归因。所以尽管在UTF-8字符串中字符数量与码点数量的关系比UTF-32更为复杂，在实际中很少会遇到有不同的情形。

优点:

　　UTF-8是ASCII的一个超集。因为一个纯ASCII字符串也是一个合法的UTF-8字符串，所以现存的ASCII文本不需要转换。为传统的扩展ASCII字符集设计的软件通常可以不经修改或很少修改就能与UTF-8一起使用。使用标准的面向字节的排序例程对UTF-8排序将产生与基于Unicode代码点排序相同的结果。（尽管这只有有限的有用性，因为在任何特定语言或文化下都不太可能有仍可接受的文字排列顺序。）UTF-8和UTF-16都是可扩展标记语言文档的标准编码。所有其它编码都必须通过显式或文本声明来指定。任何面向字节的字符串搜索算法都可以用于UTF-8的数据（只要输入仅由完整的UTF-8字符组成）。但是，对于包含字符记数的正则表达式或其它结构必须小心。UTF-8字符串可以由一个简单的算法可靠地识别出来。就是，一个字符串在任何其它编码中表现为合法的UTF-8的可能性很低，并随字符串长度增长而减小。举例说，字符值C0,C1,F5至FF从来没有出现。为了更好的可靠性，可以使用正则表达式来统计非法过长和替代值（可以查看W3 FAQ: Multilingual Forms上的验证UTF-8字符串的正则表达式）。

缺点:

　　因为每个字符使用不同数量的字节编码，所以寻找串中第N个字符是一个O(N)复杂度的操作 — 即，串越长，则需要更多的时间来定位特定的字符。同时，还需要位变换来把字符编码成字节，把字节解码成字符。

 

参考：

[http://www.cnblogs.com/skynet/archive/2011/05/03/2035105.html](http://www.cnblogs.com/skynet/archive/2011/05/03/2035105.html "http://www.cnblogs.com/skynet/archive/2011/05/03/2035105.html")

[http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html "http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html")

[http://www.joelonsoftware.com/articles/Unicode.html](http://www.joelonsoftware.com/articles/Unicode.html "http://www.joelonsoftware.com/articles/Unicode.html")

[http://www.ietf.org/rfc/rfc3629.txt](http://www.ietf.org/rfc/rfc3629.txt "http://www.ietf.org/rfc/rfc3629.txt")

 

另转载一篇非常有意思的文章加深理解

[http://www.laruence.com/2009/08/22/1059.html](http://www.laruence.com/2009/08/22/1059.html "http://www.laruence.com/2009/08/22/1059.html")

 

　　很久很久以前，有一群人，他们决定用8个可以开合的晶体管来组合成不同的状态，以表示世界上的万物。他们看到8个开关状态是好的，于是他们把这称为”字节”。

　　再后来，他们又做了一些可以处理这些字节的机器，机器开动了，可以用字节来组合出很多状态，状态开始变来变去。他们看到这样是好的，于是它们就这机器称为”计算机”。

　　开始计算机只在美国用。八位的字节一共可以组合出256(2的8次方)种不同的状态。

　　他们把其中的编号从0开始的32种状态分别规定了特殊的用途，一但终端、打印机遇上约定好的这些字节被传过来时，就要做一些约定的动作。遇上00×10, 终端就换行，遇上0×07, 终端就向人们嘟嘟叫，例如遇上0x1b, 打印机就打印反白的字，或者终端就用彩色显示字母。他们看到这样很好，于是就把这些0×20以下的字节状态称为”控制码”．他们又把所有的空格、标点符号、数字、大小写字母分别用连续的字节状态表示，一直编到了第127号，这样计算机就可以用不同字节来存储英语的文字了。大家 看到这样，都感觉很好，于是大家都把这个方案叫做 ANSI 的”Ascii”编码（American Standard Code for Information Interchange，美国信息互换标准代码）。当时世界上所有的计算机都用同样的ASCII方案来保存英文文字。

　　后来，就像建造巴比伦塔一样，世界各地的都开始使用计算机，但是很多国家用的不是英文，他们的字母里有许多是ASCII里没有的，为了可以在计算机保存他 们的文字，他们决定采用127号之后的空位来表示这些新的字母、符号，还加入了很多画表格时需要用下到的横线、竖线、交叉等形状，一直把序号编到了最后一 个状态255。从128到255这一页的字符集被称”扩展字符集”。从此之后，贪婪的人类再没有新的状态可以用了，美帝国主义可能没有想到还有第三世界国 家的人们也希望可以用到计算机吧！

　　等中国人们得到计算机时，已经没有可以利用的字节状态来表示汉字，况且有6000多个常用汉字需要保存呢。但是这难不倒智慧的中国人民，我们不客气地把那些127号之后的奇异符号们直接取消掉,规定：一个小于127的字符的意义与原来相同，但两个大于127的字符连在一起时，就表示一个汉字，前面的一个字节（他称之为高字节）从0xA1用到 0xF7，后面一个字节（低字节）从0xA1到0xFE，这样我们就可以组合出大约7000多个简体汉字了。在这些编码里，我们还把数学符号、罗马希腊的 字母、日文的假名们都编进去了，连在 ASCII 里本来就有的数字、标点、字母都统统重新编了两个字节长的编码，这就是常说的”全角”字符，而原来在127号以下的那些就叫”半角”字符了。

　　中国人民看到这样很不错，于是就把这种汉字方案叫做 “GB2312″。GB2312 是对 ASCII 的中文扩展．

　　但是中国的汉字太多了，我们很快就就发现有许多人的人名没有办法在这里打出来，特别是某些很会麻烦别人的国家领导人。于是我们不得不继续把 GB2312 没有用到的码位找出来老实不客气地用上。后来还是不够用，于是干脆不再要求低字节一定是127号之后的内码，只要第一个字节是大于127就固定表示这是一个汉字的开始，不管后面跟的是不是扩展字 符集里的内容。结果扩展之后的编码方案被称为 GBK 标准，GBK 包括了 GB2312 的所有内容，同时又增加了近20000个新的汉字（包括繁体字）和符号。后来少数民族也要用电脑了，于是我们再扩展，又加了几千个新的少数民族的字，GBK 扩成了GB18030。从此之后，中华民族的文化就可以在计算机时代中传承了。

　　中国的程序员们看到这一系列汉字编码的标准是好的，于是通称他们叫做 “DBCS”（Double Byte Charecter Set 双字节字符集）。在DBCS系列标准里，最大的特点是两字节长的汉字字符和一字节长的英文字符并存于同一套编码方案里，因此他们写的程序为了支持中文处 理，必须要注意字串里的每一个字节的值，如果这个值是大于127的，那么就认为一个双字节字符集里的字符出现了。那时候凡是受过加持，会编程的计算机僧侣 们都要每天念下面这个咒语数百遍：

　　“一个汉字算两个英文字符！一个汉字算两个英文字符……”

因为当时各个国家都像中国这样搞出一套自己的编码标准，结果互相之间谁也不懂谁的编码，谁也不支持别人的编码，连大陆和台湾这样只相隔了150海里，使用 着同一种语言的兄弟地区，也分别采用了不同的 DBCS 编码方案——当时的中国人想让电脑显示汉字，就必须装上一个”汉字系统”，专门用来处理汉字的显示、输入的问题，但是那个台湾的愚昧封建人士写的算命程序 就必须加装另一套支持 BIG5 编码的什么”倚天汉字系统”才可以用，装错了字符系统，显示就会乱了套！这怎么办？而且世界民族之林中还有那些一时用不上电脑的穷苦人民，他们的文字又怎 么办？

　　真是计算机的巴比伦塔命题啊！

　　正在这时，大天使加百列及时出现了——一个叫 ISO（国际标谁化组织）的国际组织决定着手解决这个问题。他们采用的方法很简单：废了所有的地区性编码方案，重新搞一个包括了地球上所有文化、所有字母 和符号的编码！他们打算叫它”Universal Multiple-Octet Coded Character Set”，简称 UCS, 俗称 “UNICODE”。

　　UNICODE 开始制订时，计算机的存储器容量极大地发展了，空间再也不成为问题了。于是 ISO 就直接规定必须用两个字节，也就是16位来统一表示所有的字符，对于ascii里的那些“半角”字符，UNICODE 包持其原编码不变，只是将其长度由原来的8位扩展为16位，而其他文化和语言的字符则全部重新统一编码。由于”半角”英文符号只需要用到低8位，所以其高 8位永远是0，因此这种大气的方案在保存英文文本时会多浪费一倍的空间。

　　这时候，从旧社会里走过来的程序员开始发现一个奇怪的现象：他们的strlen函数靠不住了，一个汉字不再是相当于两个字符了，而是一个！是的，从 UNICODE 开始，无论是半角的英文字母，还是全角的汉字，它们都是统一的”一个字符”！同时，也都是统一的”两个字节”，请注意”字符”和”字节”两个术语的不 同，“字节”是一个8位的物理存贮单元，而“字符”则是一个文化相关的符号。在UNICODE 中，一个字符就是两个字节。一个汉字算两个英文字符的时代已经快过去了。

　　从前多种字符集存在时，那些做多语言软件的公司遇上过很大麻烦，他们为了在不同的国家销售同一套软件，就不得不在区域化软件时也加持那个双字节字符集咒 语，不仅要处处小心不要搞错，还要把软件中的文字在不同的字符集中转来转去。UNICODE 对于他们来说是一个很好的一揽子解决方案，于是从 Windows NT 开始，MS 趁机把它们的操作系统改了一遍，把所有的核心代码都改成了用 UNICODE 方式工作的版本，从这时开始，WINDOWS 系统终于无需要加装各种本土语言系统，就可以显示全世界上所有文化的字符了。但是，UNICODE 在制订时没有考虑与任何一种现有的编码方案保持兼容，这使得 GBK 与UNICODE 在汉字的内码编排上完全是不一样的，没有一种简单的算术方法可以把文本内容从UNICODE编码和另一种编码进行转换，这种转换必须通过查表来进行。如前所述，UNICODE 是用两个字节来表示为一个字符，他总共可以组合出65535不同的字符，这大概已经可以覆盖世界上所有文化的符号。如果还不够也没有关系，ISO已经准备 了UCS-4方案，说简单了就是四个字节来表示一个字符，这样我们就可以组合出21亿个不同的字符出来（最高位有其他用途），这大概可以用到银河联邦成立 那一天吧！UNICODE 来到时，一起到来的还有计算机网络的兴起，UNICODE 如何在网络上传输也是一个必须考虑的问题，于是面向传输的众多 UTF（UCS Transfer Format）标准出现了，顾名思义，UTF8就是每次8个位传输数据，而UTF16就是每次16个位，只不过为了传输时的可靠性，从UNICODE到 UTF时并不是直接的对应，而是要过一些算法和规则来转换。受到过网络编程加持的计算机僧侣们都知道，在网络里传递信息时有一个很重要的问题，就是对于数据高低位的解读方式，一些计算机是采用低位先发送的方法，例 如我们PC机采用的 INTEL 架构，而另一些是采用高位先发送的方式，在网络中交换数据时，为了核对双方对于高低位的认识是否是一致的，采用了一种很简便的方法，就是在文本流的开始时 向对方发送一个标志符——如果之后的文本是高位在位，那就发送”FEFF”，反之，则发送”FFFE”。不信你可以用二进制方式打开一个UTF-X格式的 文件，看看开头两个字节是不是这两个字节？ 讲到这里，我们再顺便说说一个很著名的奇怪现象：当你在 windows 的记事本里新建一个文件，输入”联通”两个字之后，保存，关闭，然后再次打开，你会发现这两个字已经消失了，代之的是几个乱码！呵呵，有人说这就是联通之所以拼不过移动的原因。

　　其实这是因为GB2312编码与UTF8编码产生了编码冲撞的原因。

　　从网上引来一段从UNICODE到UTF8的转换规则：

	Unicode	 UTF-8
	0000 - 007F	0xxxxxxx
	0080 - 07FF	110xxxxx 10xxxxxx
	0800 - FFFF	1110xxxx 10xxxxxx 10xxxxxx
 

　　例如”汉”字的Unicode编码是6C49。6C49在0800-FFFF之间，所以要用3字节模板：1110xxxx 10xxxxxx 10xxxxxx。将6C49写成二进制是：0110 1100 0100 1001，将这个比特流按三字节模板的分段方法分为0110 110001 001001，依次代替模板中的x，得到：1110-0110 10-110001 10-001001，即E6 B1 89，这就是其UTF8的编码。而当你新建一个文本文件时，记事本的编码默认是ANSI,如果你在ANSI的编码输入汉字，那么他实际就是GB系列的编码方式，在这种编码下，”联通”的内码是：


	c1 1100 0001
 
	aa 1010 1010
 
	cd 1100 1101
 
	a8 1010 1000

　　注意到了吗？第一二个字节、第三四个字节的起始部分的都是”110″和”10″，正好与UTF8规则里的两字节模板是一致的，于是再次打开记事本时，记事 本就误认为这是一个UTF8编码的文件，让我们把第一个字节的110和第二个字节的10去掉，我们就得到了”00001 101010″，再把各位对齐，补上前导的0，就得到了”0000 0000 0110 1010″，不好意思，这是UNICODE的006A，也就是小写的字母”j”，而之后的两字节用UTF8解码之后是0368，这个字符什么也不是。这就 是只有”联通”两个字的文件没有办法在记事本里正常显示的原因。而如果你在”联通”之后多输入几个字，其他的字的编码不见得又恰好是110和10开始的字节，这样再次打开时，记事本就不会坚持这是一个utf8编码的文件，而会用ANSI的方式解读之，这时乱码又不出现了。好了，终于可以回答NICO的问题了，在数据库里，有n前缀的字串类型就是UNICODE类型，这种类型中，固定用两个字节来表示一个字符，无论这个字符是汉字还是英文字母，或是别的什么。如果你要测试”abc汉字”这个串的长度，在没有n前缀的数据类型里，这个字串是7个字符的长度，因为一个汉字相当于两个字符。而在有n前缀的数据类型里，同样的测试串长度的函数将会告诉你是5个字符，因为一个汉字就是一个字符。