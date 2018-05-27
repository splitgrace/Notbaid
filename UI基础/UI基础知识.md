## 分类（类别/Category）与类扩展（Extension）

### 一、分类（类别/Category）
#### 1. 适用范围
- 当你已经封装好了一个类（也可能是系统类、第三方库），不想再改动这个类了，可是随着程序功能的增加需要在类中增加一个方法，这时我们不必修改主类，只需要给原来的类增加一个分类。
- 将一个大型的类拆分成不同的分类，在不同分类中实现类别声明的方法，这样可以将一个类的实现写到多个`.m`文件中，方便管理和协同开发。
- 分类中的方法可以只声明，不实现，所以在协议不支持可选方法的时候（协议现在已经支持可选方法），通常把分类作为非正式协议使用。

#### 2. 语法格式
- 文件中的语法

```objc
@interface 主类类名 (分类类名)
// 不可以定义成员属性
@end

@implementation 主类类名 (分类类名)

@end
```
- 文件名通常为：主类名+分类名
- 调用方法时，只需要向主类引用发送消息即可

#### 3. 注意事项
- 分类中方法的优先级比原来类中的方法高，也就是说，在分类中重写了原来类中的方法，那么分类中的方法会覆盖原来类中的方法
- 分类中只能声明方法，不能添加属性变量，在运行时分类中的方法与主类中的方法没有区别
- 通常来讲，分类定义在`.h`文件中，但也可以定义`.m`文件中，此时分类的方法就变成私有方法

#### 4. 如何使用
* 定义`QLViewController+CategoryController.h`文件：

```objc
@interface QLViewController (CategoryController)
- (void)test;
@end
```
* 定义`QLViewController+CategoryController.m`文件：

```objc
@interface QLViewController (CategoryController)
- (void)test {
	NSLog(@"这是一个分类");
}
@end
```

#### 5. 虽然不能在分类（类别）中定义成员属性，但是有办法也可以让它支持添加属性和成员变量
- Category 是 Objective-C 中常用的语法特性，通过它可以很方便地为已有的类来添加方法。但是 Category 不允许为已有的类添加新的属性或者成员变量。
- 一种常见的办法是通过 runtime.h 中 `objc_getAssociatedObject/objc_setAssociatedObject` 来访问和生成关联对象。通过这种方法来模拟生成属性。
 
在 NSObject+SpecialName.h 文件中：
 
```objc
@interface NSObject (SpecialName)
@property (nonatomic, copy) NSString *specialName;
@end
```

在 NSObject+SpecialName.m 文件中：

```objc
#import "NSObject+Extension.h"
#import <objc/runtime.h>
static const void *SpecialNameKey = &SpecialNameKey;
@implementation NSObject (SpecialName)
@dynamic specialName;

- (NSString *)specialName {
	// 如果属性值是非 id 类型，可以通过属性值先构造 OC 的 id 对象，再通过对象获取非 id 类型属性
	return objc_getAssociatedObject(self, SpecialNameKey);
}

- (void)setSpecialName:(NSString *)specialName {
	// 如果属性值是非 id 类型，可以通过属性值先构造 OC 的 id 对象，再通过对象获取非 id 类型属性
	objc_setAssociatedObject(self, SpecialNameKey, spcialName, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}
@end
```

### 二、类扩展（Extension）
#### 1.适用范围
- 扩展是分类的一种特殊形式
- 也称为匿名分类

#### 2. 语法格式

```objc
@interface 主类类名 ()
@end
// 扩展通常定义在主类 .m 文件中，扩展中声明的方法直接在主类 .m 文件中实现
```

#### 3. 注意事项
- 扩展中可以声明实例变量，可以声明属性
- 因为扩展通常定义在主类的 `.m` 文件中，所以扩展声明的方法和属性通常是私有的

#### 4. 分类和扩展的区别
- 分类不可以声明实例变量，通常是公开的，文件名是：`主类名+分类名.h`
- 扩展是可以声明实例变量，是私有的，文件名是：`主类名_扩展标识.h`，在主类的 `.m` 文件中 `#import` 该头文件

#### 5. 使用方法
- 方式一：以单独的文件定义 `QLViewController_ExtensionController.h` 文件

```objc
#import "QLViewController.h"

@interface QLViewController ()
@property (nonaomic, copy) NSString *stringExtension;
- (void)testExtension;
@end
```

- 方式二：在主类的 `.m` 文件中定义 `QLViewController.m`文件，此种最常用

```objc
#import "QLViewController.h"

@interface QLViewController ()
@property (nonatomic, copy) NSString *stringExtension;
- (void)testExtension;
@end

@implementation QLViewController
- (void)testExtension {
	self.stringExtension = @"字符串";
	NSLog(@"定义的属性String是:%@", self.stringExtension);
}
@end
```
