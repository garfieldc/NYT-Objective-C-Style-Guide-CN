# NYT-Objective-C-Style-Guide-CN
==============================  
*NYTimes Objective-C Style Guide Chinese Edition*

##代码风格指南
本指南描述了New York Times的iOS团队所使用的编码约定。
 
##简介
下面列出了一些来自于Apple与编码风格指南相关的文档。如果在本风格指南有未被提及的内容，那么通常来说下面这些文档当中会对此有非常详细的描述：  
>[The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)  
[Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)  
[Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)  
[iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)  
 
##目录

 
###点符号(Dot-Notation Style, ".")语法
保持使用"."来访问以及改变对象的属性。括号("[]")推荐用来访问实例的其他部分。
 
####实例:  
	view.backgroundColor = [UIColor orangeColor];  
	[UIApplication sharedApplication].delegate;

####避免:  
    [view setBackgroundColor:[UIColor orangeColor]];  
    UIApplication.sharedApplication.delegate;
 
###空白
使用4个空格，而不是一个Tab来进行代码的缩进。请确保在Xcode的preference里对此予以设置。  
方法体以及其他代码块(if/else/while/switch等)所使用的大括号("{}")，其起始"{"应当与该语句出现在同一行，但是其结束"}"必须在新的一行中出现。
 
####实例：
	if (user.isHappy) {  
	    //Do something  
	}  
	else {  
	    //Do something else  
	}  
 
方法与方法之间应当以一个空行分隔，以使代码在看起来既清晰又有条理。一个方法内部的空白行用于将方法中相互分离的功能加以分隔，但是更合理的做法是将这些相互分离的功能变成新的方法。
在所有的实现当中，每一个@synthesize或@dynamic都要占据一个单独的行。
 
###条件判断
条件体在任何情况下都应当以大括号包围("{}")，即便是从语法角度上来说该条件提完全不需要使用大括号(例如仅有一条语句的条件体)的情况下也是如此，这样做的目的是为了避免发生[错误](https://github.com/NYTimes/objective-c-style-guide/issues/26#issuecomment-22074256)。这些错误当中包括向if语句当中加入第二行代码，并且期望其成为if语句的一部分，在这种情况下，或者我们加上"{}"以使之符合我们的预期，或者使得这一新增加的语句游离于条件体之外，从而带来非预期的后果。[更严重的缺陷在于](http://programmers.stackexchange.com/a/16530)，如果我们不使用"{}"来包围条件体代码，一旦我们注释掉这个条件体内的语句，那么紧跟着这个条件体的下一条语句就会无意之间成为这个if语句的一部分。另外，这种风格也同时适用于其他类型的条件语句，并且使得代码更加易于检查。
 
####实例：
	if (!error) {  
	    return success;}
 
####避免:
	if (!error)
	    return success;
 
####或者
	if (!error) return success;
 
###三元运算符
三元运算符"?"必须在能够提高代码可读性，或者是带来更整洁代码的情况下才能使用。通常只在使用单一条件判断的情况下三元运算符，多重条件判断时，或者使用if语句以便使代码更易理解，或者是将其重构为实例变量。
 
####实例:
	result = a > b ? x : y;
 
####避免：
	result = a > b ? x = c > d ? c : d : y;
 
###错误处理
 
如果方法以形参的方式返回错误时，改为直接返回错误值，不必为此引入错误变量。即要根据返回值判断是否错误，而非根据错误变量来直接判断错误是什么。
 
####实例:
	NSError *error;
	 
	if (![self trySomethingWithError:&error]) {
	    // Handle Error
	}
 
####避免:
	NSError *error;
	 
	[self trySomethingWithError:&error];
	if (error) {
	    // Handle Error
	}
 
上述约定的出发点是有些Apple的API会在成功执行时，在错误参数不为空的情况下向其写入垃圾值，因此针对错误值进行条件判断可能会使得程序走入错误的分支，从而导致后续的程序崩溃。而实际上我们也不能避免自己编写的代码不会出现向错误参数内写入垃圾值。
 
###方法
在方法的命名中，其作用范围(-/+符号)后保留一个空格。方法的每一个段之间也要保留一个空格。
 
####实例：
	- (void)setExampleText:(NSString *)text image:(UIImage *)image;
 
###变量
变量的命名要尽可能地描述其本来的含义，除了在for()循环中作为循环变量之外，不使用单字母变量名。
 
“\*”作为指针的标识，应当从属于变量，例如，应当写成NSString \*text，而非NSString\* text或者是NSString \* text，常量除外。
 
在任何情况下都尽可能地使用Property定义来代替那些直接的实例变量。只有在初始化方法(例如init，initWithCoder等)，dealloc方法，以及定制化的setter和getter内部使用实例变量。有关在初始化方法和dealloc方法里使用直接的实例变量更详细的信息，请参考[这里](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)。
 
####实例:
	@interface NYTSection: NSObject
	@property (nonatomic) NSString \*headline;
	@end
 
####避免:
	@interface NYTSection : NSObject {NSString \*headline;}
 
###命名
尽可能坚持使用Apple的命名约定，尤其是那些涉及到[内存管理规则](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)([NARC](http://stackoverflow.com/a/2865194/340508))的变量和方法。
 
长且具有自描述能力的变量和方法名是比较适宜的命名方式。
####实例:
	UIButton *settingsButton;
 
####避免：
	UIButton *setBut;
 
在类名称及变量名称上使用三个字母的前缀(例如NYT)，但是在Core Data实体的命名时可以忽略这一前缀。附带说明一下，由于在Objective-C里不支持命名空间，因此如果使用的Library里有重名的类或者常量等等就会引发很多的问题，加上这样一个前缀就相当于定义了命名空间；Core Data是依附于特定应用的，只要是之前的命名没有重复，就不会存在由于命名空间所导致的重名问题。常量的命名在遵从camel-case的情况下，单词大写(首字母大写或全词大写)，并且加上与之相关的类名作为前缀，以使其更加清晰。
 
####实例:
	static const NSTimeInterval 	NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
 
####避免:
	static const NSTimeInterval fadetime = 1.7;
 
属性及局部变量使用camel-case，并且首字母小写。
 
实例变量使用camel-case，并且首字母小写，以"_"作为前缀。这与LLVM自动合成的实例变量命名方式保持一致。如果LLVM能够自动合成变量，那么可以直接使用。
 
####实例:
	@synthesize descriptiveVariableName = _descriptiveVariableName;

####避免:
	id varnm;
 
###注释
注释用于在必要时说明为何某一段代码要完成某一功能。如果注释不能够保持最新，就请将其删除。
 
要避免使用大段的注释，代码应当成为自己的说明文档，我们应当仅在必要时间隔性地为其加上简短的注释。而这些代码并非用于生成文档。
 
###init以及dealloc
dealloc语句要放在类的实现的最前面，仅次于@synthesize和@dynamic的位置。init方法在任何一个类的实现里都应当放在仅次于dealloc后面的位置。
 
init方法应当遵循如下的结构：
	- (instancetype)init {
	    self = [super init]; // or call the designated initalizer
	    if (self) {
	        // Custom initialization
	    }
 	
	    return self;}
 
###字面值
创建NSString, NSDictionary, NSArray, NSNumber类的不可变实例时，应当使用字面值。特别需要注意的是，nil不能传递给NSDictionary和NSArray字面值，这样做会导致程序的崩溃。
 
####实例:
	NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
	NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
	NSNumber *shouldUseLiterals = @YES;NSNumber *buildingZIPCode = @10018;
 
####避免:
	NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
	NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
	NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
	NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
 
###CGRect函数
访问CGRect的x，y，height，以及width值时，不要直接访问其结构体成员，而是通过[CGGeomtry](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html)所提供的各种[函数](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html)进行访问。根据Apple公司的CGGeometry参考：
 
>本参考中所有以CGRect数据结构作为输入的函数，毫无疑问是为了使得那些Rectangle在进行计算并获得结果之前能够被标准化。出于该原因，应用应当避免直接对CGRect数据结构当中所存储的数据进行读写。作为替代，使用本参考所描述的函数去操纵rectangle，并且获取其各类属性值。
 
####实例:
	CGRect frame = self.view.frame;
 
	CGFloat x = CGRectGetMinX(frame);
	CGFloat y = CGRectGetMinY(frame);
	CGFloat width = CGRectGetWidth(frame);
	CGFloat height = CGRectGetHeight(frame);
 
####避免:
	CGRect frame = self.view.frame;
 
	CGFloat x = frame.origin.x;
	CGFloat y = frame.origin.y;
	CGFloat width = frame.size.width;
	CGFloat height = frame.size.height;
 
###常量
常量的使用会优于那些在代码中直接插入的字符串或者数字。因为这些常量易于在代码各处重现那些常用的变量，并且可以在无需查询与替换操作的情况下快速进行改变。
 
除非是将常量明确地作为宏，否则必须使用static保留字来声明常量，而非使用#define。
 
####实例:
	static NSString * const NYTAboutViewControllerCompanyName = @"The New York Times Company";
	 
	static const CGFloat NYTImageThumbnailHeight = 50.0;
 
###避免:
	#define CompanyName @"The New York Times Company"
	 
	#define thumbnailHeight 2
 
###枚举类型
使用enum时，建议使用那些新的固定基础类型来进行定义。这些类型一方面有更好的代码完成度，另一方面也具备更强壮的类型检查。SDK现在已经包含有一个名为NS_ENUM()的宏鼓励与促进对于固定基础类型的使用。
 
####实例:
	typedef NS_ENUM(NSInteger, NYTAdRequestState) {
	    NYTAdRequestStateInactive,
	    NYTAdRequestStateLoading};
 
###私有属性
私有属性在类的实现文件中类的扩展部分(anonmyous categories)中加以声明。已经被命名的categories，诸如NYTPrivate或者private都不可使用，除非是对其他类进行扩展。
 
####实例:
	@interface NYTAdvertisement ()
	 
	@property (nonatomic, strong) GADBannerView *googleAdView;@property (nonatomic, strong) ADBannerView *iAdView;@property (nonatomic, strong) UIWebView *adXWebView;
	 
	@end
 
###图片命名
Project当中的图片应当依据统一的规则进行命名，以此保证团队及开发者都可以正确理解其含义。使用camel case进行命名，为其所给出的名称应当充分反映其含义及用途，如果涉及到已有的自定义对象或属性，则在其后加上无前缀的类型或属性名，再其次是对其颜色和/或位置的进一步描述，最后是其状态。
 
####实例:
 
*  RefreshBarButtonItem  /  RefreshBarButtonItem@2x  and  RefreshBarButtonItemSelected  /  RefreshBarButtonItemSelected@2x
*  ArticleNavigationBarWhite  /  ArticleNavigationBarWhite@2x  and  ArticleNavigationBarBlackSelected  /  ArticleNavigationBarBlackSelected@2x .
 
用于相似用途的图片应当进行分组保存到图片目录当中各自单独的组中。
 
###布尔值
由于nil在布尔类型当中被解释为NO，因此在条件比较时完全不必对nil进行比较。任何值都不要直接与YES进行条件比较，而应当是直接判断，在BOOL当中的YES定义为1，相应的，一个BOOL值最多由8位组成，这样的逻辑比较很容易出现非预期的情况。
 
以上的约定能够使得在多个objc文件之间有良好的一致性，并且使得代码有更好更清晰的表达。
 
####实例:
	if (!someObject) {}
 
####避免:
	if (someObject == nil) {}
 
####布尔值的实例:
	if (isAwesome)
	if (![someObject boolValue])
 
####避免:
	if ([someObject boolValue] == NO)
	if (isAwesome == YES) // Never do this.
 
如果一个BOOL类型的属性，其名称已经使用形容词进行定义，则其属性名称可以省略"is"前缀，但是为其指定的get访问器名称仍然要依据惯例保持"is"前缀。
 
	@property (assign, getter=isEditable) BOOL editable;
 
上面的说明及例子来自于[Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE)。
 
###Singletons
Singleton对象要按照进程安全的模式来定义其共享实例。
 
	+ (instancetype)sharedInstance {
	   static id sharedInstance = nil;
	 
	   static dispatch_once_t onceToken;
	   dispatch_once(&onceToken, ^{
	      sharedInstance = [[self alloc] init];
	   });
 	
	   return sharedInstance;}
 
这样有助于防止那些[可能由此引发的，并且在特定情况下会出现的大量崩溃的发生](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)。
 
###Xcode Project
为防止文件存放散乱，物理文件应当与Xcode项目文件时刻保持同步。凡是在Xcode当中建立的Group，都应当能够映射到文件系统的文件夹。为了获得更清晰的项目结构，代码文件要按照其类型及用途进行分组。
 
如果可能，保持Target的Build Settings当中的"Treat Warnings as Error"选项打开，并且允许尽可能多的附加警告。如果确实需要忽略某一特定的警告，请使用[Clang's Pragma特性](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas)。
 
 
其他Objective-C代码风格指南
 
如果认为本代码风格指南不符合品味，可以了解一下其他的代码风格指南：
>[Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)  
[GitHub](https://github.com/github/objective-c-conventions)  
[Adium](https://trac.adium.im/wiki/CodingStyle)  
[Sam Soffes](https://gist.github.com/soffes/812796)  
[CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)  
[Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)  
[Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)  
 
 