## id万能指针

- id可以指向任何OC对象



## 自定义构造方法

- 必须以init开头，后面接的单词首字母大写

### 循环Retain

一方不进行内存管理，使用assian;

自动释放池只自动释放一次

## Block

```objective-c
#import <Foundation/Foundation.h>

void (^blockXXX)()=^{
     NSLog(@"Hello, World!");
};

void (^sum1)(int,int) = ^(int a,int b){
    NSLog(@"%d",a+b);
};

int (^sum3)(int,int) = ^(int a,int b){
    return a+b;
};

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        
        blockXXX();
        sum1(1,249);
        int c = sum3(1,249);
        NSLog(@"你是%d",c);
    }
    return 0;
}
// 真是奇怪的语法
```

## 类别，分类

单独的`.h`和`.m`文件，单独的声明，直接引入。不能声明属性

```objective-c
#import <Foundation/Foundation.h>
#import "Person.h"
#import "Person+eat.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        id p = [[Person alloc] init];
        [p eat];
        [p play];
    }
    return 0;
}
```

```objective-c
#import "Person.h"

@interface Person (eat)

-(void) eat;

@end
```

```objective-c
#import "Person+eat.h"

@implementation Person (eat)
-(void)eat{
    NSLog(@"哈哈哈");
}
@end
```



## 拓展

可以声明属性

```objective-c
#import "Person.h"

@interface Person ()
{
    NSString *_name;
}

-(void) show;
@end

```



```objective-c
#import "Person.h"
#import "Person_Test.h"

@implementation Person

-(void)show{
    NSLog(@"拓展");
}
@end

```

建议的做法是这样:

```objective-c

#import "Person.h"

@interface Person()
{
 int sum;
}

-(void) show;

@end

@implementation Person

-(void)play{
    NSLog(@"%d岁的人在踢足球",_age);
}

-(void)show{
    NSLog(@"show，mdzz");
}

@end
```



## 协议

不能声明属性，只能声明方法，而且声明的方法必须全部实现



```objective-c

#import <Foundation/Foundation.h>

@protocol Mdzz <NSObject>

-(void) eat;

-(void) play;
@end
```

```objective-c

#import "Person.h"

@implementation Person

-(void)eat{
    NSLog(@"哈哈哈，吃吃");
}

-(void) play{
    NSLog(@"哈哈哈，玩玩");
}

@end

```

采用一个协议

```objective-c
#import <Foundation/Foundation.h>
#import "Mdzz.h"

@interface Person : NSObject <Mdzz>

@end
```

采用两个协议

```objective-c
#import <Foundation/Foundation.h>
#import "Mdzz.h"

@interface Person : NSObject <Mdzz,Mdzz>

@end
```

可以使用

```objective-c
@optional
-(void) study;

@required
-(void) travlle;;
```

来告诉主类，是否一定要实现协议方法。



## NSString方法使用

替换字符

```objective-c
 // 替换字符
        NSString * str = @"i am     a        chinese";
        NSRange range = NSMakeRange(0, str.length);
        NSString *replaceStr = [str stringByReplacingOccurrencesOfString:@"\\s+" withString:@" " options:1024 range:range];
        NSLog(@"%@",replaceStr);
        
        // 遍历字符
        str.intValue;
        str.doubleValue;
        // 从oc字符串转换为c语言字符串
        str.UTF8String;
```

