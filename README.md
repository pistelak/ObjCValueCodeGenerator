# ObjCValueCodeGenerator
Ruby DSL for generating Obj-C code 

Are you confused about value types and reference types? Look at: 
- Values and objects in programming languages, B. J. MacLennan. 1982. Values and objects in programming languages. SIGPLAN Not. 17, 12 (December 1982), 70-79.

# WIP

Schema example: 
```Ruby
model "Person" do
  property "name", type: "NSString *"
  property "age", type: "NSNumber *"
  property "height", type: "NSNumber *"
  property "weight", type: "NSNumber *"
  property "alive", type: "BOOL", default: "YES", getter: "isAlive"
end
```
Result: 
```ObjC
@import Foundation; 

@interface PersonBuilder : NSObject

@property (nonatomic, copy) NSString *name;
@property (nonatomic, copy) NSNumber *age;
@property (nonatomic, copy) NSNumber *height;
@property (nonatomic, copy) NSNumber *weight;
@property (nonatomic, getter=isAlive) BOOL alive;

@end

@interface Person : NSObject <NSCopying>

@property (nonatomic, copy, readonly) NSString *name;
@property (nonatomic, copy, readonly) NSNumber *age;
@property (nonatomic, copy, readonly) NSNumber *height;
@property (nonatomic, copy, readonly) NSNumber *weight;
@property (nonatomic, readonly, getter=isAlive) BOOL alive;

- (instancetype)init NS_UNAVAILABLE;
+ (instancetype)makeWithBuilder:(void (^)(PersonBuilder *))updateBlock;
- (instancetype)update:(void (^)(PersonBuilder *))updateBlock;

// - (BOOL)isEqualToPerson:(Person *)other;

@end
```

It generates also code for `builder` pattern (at least for now).


- Based on https://spin.atomicobject.com/2015/11/02/objective-c-value-objects-mantle/
