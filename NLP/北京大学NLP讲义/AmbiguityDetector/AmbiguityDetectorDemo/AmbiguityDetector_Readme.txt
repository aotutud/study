AmbiguityDetector.exe程序说明文件


功能概述：

给定一个上下文无关文法规则集R，对用户指定的由词或短语标记组成的排列格式P1,P2,...Pn进行自动分析，考察每个格式（Pi）可能形成的句法结构树。

对任意的Pi，程序分析结果共有4种情况：
（1） Pi 不能形成一棵树；
（2） Pi 只能形成一棵树—— Pi 排列式在R语言模型下无歧义；
（3） Pi 能形成两棵以上的树T1，T2，... Tm，这些树的根节点只有一种，即属相同短语类 —— Pi 为内含型歧义格式
（4） Pi 能形成两棵以上的树T1，T2，... Tm，这些树的根节点不只一种，即属不同短语类 —— Pi 为外显型歧义格式

AmbiguityDetector.exe程序对所有排列式的上述4种可能的分析结果进行计数，并列出全部分析结果树。根据统计结果，可以
（1）初步了解一种排列式的句法结构歧义程度及其特点。
（2）评估不同的CFG规则集在未给出约束条件时的整体歧义程度差异。


使用说明：

一

config.ini 配置文件中指定了程序运行所需的文件，以及参数设置。具体说明如下： （/* ... */ 之间为说明内容）

TAGS=Data\tags.txt    /* tags.txt文件中指定用户希望考察的短语或词标记。这些标记形成的全排列式是程序的分析对象 */
SEQUENCES=Data\sequences.txt  /* sequences.txt文件中直接列出用户感兴趣的排列式，每个排列式占一个文本行，每个排列式所含短语或词标记最多为 5 个 */
RULES=Data\rules.txt  /* rules.txt文件是程序分析所依赖的上下文无关文法规则。用户可对规则进行编辑 */
RESULT=AmbiguityReport.txt  /* 指定保存统计结果的文件名 */
ITEMLENGTH=short /* ITEMLENGTH可以有两种取值：ITEMLENGTH=short值表示tags.txt文件中的标记在全排列时形成3项排列，ITEMLENGTH=long值表示tags.txt文件中的标记在全排列时形成4项和5项排列 */

3项排列式如：

ap  np  vp
vp  vp  ap
... 


4项和5项排列式如：  

ap  np u<的> vp
vp c<和> vp u<的> ap

 （4项或5项排列式是在3项排列式中插入“的”或“和”形成的新的排列式） 


二

tags.txt文件格式说明：

TagSetBegin
	 np,vp,ap
TagSetEnd

其中 TagSetBegin和TagSetEnd之间的一行是用户可编辑的区域。用户在这里指定希望考察的短语标记或词性标记。
比如填入“np,vp,ap”表示用户要考察这三个短语标记形成的排列式。它们可能形成的3项排列式有27个。比如：
	np np np
	np np vp
	np np ap
	……

这三个标记可能形成的4项和5项排列式有216个（27×8）。比如：
	np u<的> np np 
	np u<的> np vp 
	np u<的> np ap
	……

tags.txt文件内容内容若为空，程序返回提示信息 "No defined part-of-speech tag(s) and phrase tag(s)!       "

三

sequences.txt文件格式说明：

sequences.txt文件中每一个文本行是一个短语类或词类标记串。程序可以考察这些特定的串生成歧义结构分析树的可能性。

比如：

np vp np u<的> np 是 一般主谓结构（SVO，或 np vp np 序列）在“的”前关系化的格式
np np vp u<的> np 是 主谓谓语结构（OSV，或 np np vp 序列）在“的”前关系化的格式


类似的考察（对比组）：

n v n   论文指导老师
v n n   指导论文老师

n v u<的> n   文学研究的春天
v n u<的> n   研究文学的春天

n a u<的> n  颜色鲜艳的花朵
a n u<的> n  长头发的学生

sequences.txt文件内容若为空，程序返回提示信息 "No sequences defined!	"

四

rules.txt文件格式说明：

关键字CFGRuleSetBegin 表示规则开始
关键字CFGRuleSetEnd 表示规则结束
规则存放在CFGRuleSetBegin和CFGRuleSetEnd之间。每条规则占一个文本行。

每条规则都由5部分组成
（1）&& 规则开始标识符
（2）{***} 规则编号
（3）x->y  上下文无关文法产生式
（4）:: 分隔符
（5）$.内部结构=**** 说明当前产生式所描述的短语组合的内部结构 

如果rules.txt文件无规则或格式有误，程序会返回错误提示信息。


五

AmbiguityDetector.exe程序所在目录下必须有config.ini文件和Data文件夹。
Data文件夹下必须有rules.txt，tags.txt, sequences.txt 三个文件。

双击AmbiguityDetector.exe后程序启动。点击菜单“歧义格式分析”下的菜单项“生成歧义格式统计报告”，
在程序所在目录下生成AmbiguityReport.txt文件。打开该文件可查看统计结果。

用户可以在程序窗口中打开Data目录下的三个文件，编辑修改其中的内容。保存后，再点击菜单“歧义格式分析”下的菜单项“生成歧义格式统计报告”，程序将根据用户修改后的内容，生成新的统计报告。


六

标记符号说明：

// 短语标记
zj // 整句
fj // 复句
dj // 小句，主谓结构短语
np // 名词性短语
vp // 动词性短语
ap // 形容词性短语
dp // 副词性短语
pp // 介词性短语
tp // 时间词性短语
sp // 处所词性短语
mp // 数量词性短语
mcp // 数词性短语

// 词性标记
a  // 形容词
b  // 区别词
c  // 连词
d  // 副词
e  // 叹词
f  // 方位词
m  // 数词
n  // 名词
p  // 介词
q  // 量词
r  // 代词
s  // 处所词
t  // 时间词
u  // 助词
v  // 动词
w  // 标点
y  // 语气词
z  // 状态词

! // 中心词标记

詹卫东 
2011.11.4