SecureMappingKit 
==============================

[![Twitter](https://img.shields.io/badge/contact-@leverdeterre-green.svg)](http://twitter.com/leverdeterre)
[![License MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/leverdeterre/SecureMappingKit/blob/master/LICENSE)
[![Cocoapods](http://img.shields.io/cocoapods/v/SecureMappingKit.svg)](https://github.com/leverdeterre/SecureMappingKit)
[![Travis](http://img.shields.io/travis/leverdeterre/SecureMappingKit.svg)](https://travis-ci.org/leverdeterre/SecureMappingKit)

Securize your mapping between your JSON and your object.
What is security ? The library is in charge of the convertion of the JSON objects to your desired types. 
If your are expected a NSDate and the JSON gives you a NSString, SecureMappingKit transforms it into the exepected class, using NSValueTransformer.

Actual tranformers : 
- [x] NSNumberTransformer,
- [x] NSBooleanNumberTransformer, to be sure to have a NSNumber of a boolean 
- [x] NSStringTransformer,
- [x] NSURLTransformer,
- [x] NSArrayTransformer,
- [x] NSDateTransformer,
- [x] NSDecimalTransformer

## +_+ 
- [x] ThreadSafe,
- [x] Memory usage, DateFormaters and ValueTransformers are allocated once and stored on the Thread dictionary.
- [x] Secure mappings,
- [x] lots of unit tests

## Get SecureMappingKit (Soon)

If you use Cocoa Pods, you can get SecureMappingKit by adding to your podfile `pod 'SecureMappingKit', '~>0.0.1b1'`. 

##Using SecureMappingKit
### On a NSDictionnary

```objective-c
- (id)objectForKey:(id)aKey expectedClass:(Class)expectedClass;
- (id)objectForKey:(id)aKey expectedClass:(Class)expectedClass withTransformerClass:(Class)transformerClass;
- (id)objectForKey:(id)aKey withTransformerBlock:(JMOTransformerBlock)transformerBlock;

- (NSNumber *)numberForKey:(id)aKey;
- (NSNumber *)boolNumberForKey:(id)aKey;
- (NSDecimalNumber *)decimalNumberForKey:(id)aKey;
- (NSString *)stringForKey:(id)aKey;
- (NSURL *)urlForKey:(id)aKey;
- (NSArray *)arrayForKey:(id)aKey;
- (NSDate *)dateForKey:(id)aKey usingDateFormat:(NSString *)dateFormat;
```

Configure optional values
```objective-c
[SecureMappingKit setDateFormatterForcedlocale:[NSLocale localeWithLocaleIdentifier:@"fr_FR"]];
[SecureMappingKit setDateFormatterForcedTimeZone:[NSTimeZone timeZoneWithAbbreviation:@"GMT"]];
[SecureMappingKit setLogEnable:YES];
```

### On other classes ?
```objective-c
[NSNumber numberFromObject:obj];
[NSNumber boolNumberFromObject:@"true"];
[NSNumber boolNumberFromObject:@"1"];
[NSNumber boolNumberFromObject:@(1)];
[NSDecimalNumber decimalNumberFromObject:@"1,1"];
[NSString stringFromObject:@(1)];
[NSURL urlFromObject:@"https://developer.apple.com/design/"];
[NSDate dateFromObject:@"07/26/1982" usingDateFormat:@"MM/dd/yyyy"]
```

###Using SecureMappingKit, implement SecureDecoding protocol

```objective-c
- (void)decodeObjectWithDictionary:(NSDictionary *)dict
{
    NSString *identifier = [dict objectForKey:@"id" expectedClass:NSString.class];

    //NSNumber has boolean
    NSNumber *isActive = [dict objectForKey:@"isActive" 
                              expectedClass:NSNumber.class 
                       withTransformerClass:NSBooleanNumberTransformer.class];
                            
    //NSNumber
    NSNumber balance = [dict objectForKey:@"balance" expectedClass:NSNumber.class];
    
    //@{@"url":@"https://developer.apple.com/design/"}
    NSURL *url = [dict  objectForKey:@"url" expectedClass:NSURL.class];
    
    NSDate *date = [dict  dateObjectForKey:@"date" withDateFormat:@"MM/dd/yyyy"];
}
```


