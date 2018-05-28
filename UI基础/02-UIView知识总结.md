## UIView简介
### 一、UIView的常见属性
- NSArray *subviews
	- 拿到所有的子控件
	- 数组元素的顺序决定着子控件的显示层级顺序（下标越大的，越显示在上面）
- UIView *superview
	- 父控件
	- 子控件可以根据这个属性拿到对应的父控件

```objc
@property(nullable, nonatomic,readonly) UIView *superview;
@property(nonatomic,readonly,copy) NSArray<__kindof UIView *> *subviews;
```

### 二、UIView的常见方法
- 排列控件
- 拿到子控件

```objc
- (void)removeFromSuperview;
- (void)insertSubview:(UIView *)view atIndex:(NSInteger)index;
- (void)exchangeSubviewAtIndex:(NSInteger)index1 withSubviewAtIndex:(NSInteger)index2;

- (void)addSubview:(UIView *)view;
- (void)insertSubview:(UIView *)view belowSubview:(UIView *)siblingSubview;
- (void)insertSubview:(UIView *)view aboveSubview:(UIView *)siblingSubview;

- (void)bringSubviewToFront:(UIView *)view;
- (void)sendSubviewToBack:(UIView *)view;

- (nullable __kindof UIView *)viewWithTag:(NSInteger)tag;
```