title: XML Parsing - IOS
date: 2016-02-22 14:36:27
tags:

 - IOS
 - XML

---

# 什么是 XML

> 如果不知道它的产生，何谓使用它呢？

## 认识和了解 XML


  - XML 是可扩展标记语言（Extensible Markup Language）的缩写，其中的 标记（markup）是关键部分。您可以创建内容，然后使用限定标记标记它，从而使每个单词、短语或块成为可识别、可分类的信息。您创建的文件，或文档实例 由元素（标记）和内容构成。

  - 当从打印输出读取或以电子形式处理文档时，元素能够帮助更好地理解文档。元素的描述性越强，文档各部分越容易识别。

  - 标记语言从早期的私有公司和政府制定形式逐渐演变成标准通用标记语言（Standard Generalized Markup Language，SGML）、超文本标记语言（Hypertext Markup Language，HTML），并且最终演变成 XML。SGML 比较复杂，HTML（实际上仅是一组元素集）在识别信息方面不够强大。XML 则是一种易于使用和易于扩展的标记语言。

  - 可以使用 XML 创建自己的元素，从而能够更精确地表示自己的信息。您可以在文档内部识别每个部分，而不是将文档看作仅由标题和段落组成。为了提高效率，您可能需要定义数量一定的元素，并统一使用它们。（您可以在文档类型定义（Document Type Definition， DTD ）或模式 （schema）中定义元素，稍后我将对此进行简要的描述）。一旦习惯使用 XML 之后，就可以在构建文件时尝试处理元素名称。

  - 用来标记数据、定义数据类型，是一种允许用户对自己的标记语言进行定义的源语言。 它非常适合万维网传输，提供统一的方法来描述和交换独立于应用程序或供应商的结构化数据。是Internet环境中跨平台的、依赖于内容的技术，也是当今处理分布式结构信息的有效工具。早在1998年，W3C就发布了XML1.0规范，使用它来简化Internet的文档信息传输。

  如果你想学习XML，[点这里 - XML 新手入门基础知识](http://www.ibm.com/developerworks/cn/xml/x-newxml/)

## XML 长什么样呢？
 ![image1](http://7xiwtw.com1.z0.glb.clouddn.com/WYQp_xml_example.png)

# 解析 XML

> 这里不做XML的生成，只是做解析。

## XML 解析的过程

**IOS 使用的是Delegate 来实现 XML 的解析 `<NSXMLParserDelegate>`**

```{bash}

//开始解析步骤
 1. - (void)parserDidStartDocument:(NSXMLParser *)parser

//遇到一个 <tag>
 2. - (void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
      namespaceURI:(NSString *)namespaceURI
     qualifiedName:(NSString *)qName
        attributes:(NSDictionary *)attributeDict

//找到<tag>中的内容
 3. - (void)parser:(NSXMLParser *)parser foundCharacters:(NSString *)string

//遇到一个 </tag>
 4. - (void)parser:(NSXMLParser *)parser didEndElement:(NSString *)elementName
      namespaceURI:(NSString *)namespaceURI
     qualifiedName:(NSString *)qName

//解析中出现了错误
 5. - (void)parser:(NSXMLParser *)parser parseErrorOccurred:(NSError *)parseError

//解析完成的最后收拾
 6. - (void)parserDidEndDocument:(NSXMLParser *)parser

```

**XML 解析流程说明**


  - 从 1 开始，遇到一个`<tag>`, 调用2方法，遇到一个`内容`调用3方法，遇到一个`</tag>`调用4的方法，如果出现了`解析错误`就会调用5方法。知道`解析结束`掉用6方法。
      大概就是这么一个流程。

  - 2，3，4，5的方法的调用是多次的，依赖于xml的内容；1和6在整个解析过程中各自只有一次。


## 解析使用的例子:

[XML](http://www.raywenderlich.com/demos/weather_sample/weather.php?format=xml)

# 解析XML成Dictionary － 参考书上和自己的问题所作的类

> 这两个文件可以直接 `食用` (千万不要真的把它给吃了，要消化啦。。。)

 - \*.m 文件里面有delegate处理的使用, 有conform `<NSXMLParserDelegate>` 的说明

**XMLParser.h**

```{bash}

//
//  XMLParser.h
//  Test
//
//  Created by wuyingqiang on 7/19/15.
//  Copyright (c) 2015 wuyingqiang. All rights reserved.
//

/*
 * first, set the prepare phase.
 * second, start xml parse.
 * finally, set the block of operation when xml parse was done.
 * ------------------------------------------------------------
 *
 * <FUNCTIONALITY>: Parsing XML file into dictionary.
 *
 * this class just give you several class method to complete your goal.
 * you don't need to set anything for this class, just use one method to deal with your problem.
 *
 * ---------------------------------------------------------------------------------------------
 *
 * <CLASS METHOD>:
 *      +xmlParseByData:completeBlock:
 *      +xmlParseByData:completeBlock:dictionaryCloseInArray:
 *      +xmlParseByURL:completeBlock:
 *      +xmlParseByURL:completeBlock:dictionaryCloseInArray:
 *      +xmlParseWithURLByString:completeBlock:
 *      +xmlParseWithURLByString:completeBlock:dictionaryCloseInArray:
 *      +xmlParseWithXMLParser:completeBlock:
 *      +xmlParseWithXMLParser:completeBlock:dictionaryCloseInArray:
 *
 * ---------------------------------------------------------------------------------------------
 *
 * <PARAMETER>:
 *
 *      - data: (NSData *), the data from server you have already download.
 *
 *      - url : (NSURL *), the url of xml in server.
 *
 *      - stringOfURL : (NSString *), the string of URL.
 *
 *      - parser: (NSXMLParser *), XML serializer you have download from server,and convertd. saved in NSXMLPareser.
 *
 * ------------------------------------------------------------------------------------------------------------------
 *
 * <EXAMPLE>:
 *      
 *     [[[XMLParser alloc] init] xmlParseByData:data
 *                                completeBlock:^(NSDictionary *data) {
 *
 *                                  // yet completed the parsing, data was saved in the
 *                                  // dictionary,which you have parsing done
 *
 *                                  }];
 *
 * -----------------------------------------------------------------------------------------------------------------
 *
 * <CONSUMMER>:
 *
 *    - the block's data is the result of parsing.
 *
 * <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< Having Fun - END >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>-------------
 *
 */

#import <Foundation/Foundation.h>

@interface XMLParser : NSObject

+ (void)xmlParseByData:(NSData *)data
         completeBlock:(void (^)(NSDictionary *data)) parseCompleted;
+ (void)xmlParseByData:(NSData *)data
         completeBlock:(void (^)(NSDictionary *data)) parseCompleted dictionaryCloseInArray:(BOOL)close;
/* parsing xml use data from server
 *
 */

+ (void)xmlParseByURL:(NSURL *)url
        completeBlock:(void (^)(NSDictionary *data)) parseCompleted;
+ (void)xmlParseByURL:(NSURL *)url
        completeBlock:(void (^)(NSDictionary *data)) parseCompleted dictionaryCloseInArray:(BOOL)close;
/* parsing xml url
 *
 */

+ (void)xmlParseWithURLByString:(NSString *)stringOfURL
         completeBlock:(void (^)(NSDictionary *data)) parseCompleted;
+ (void)xmlParseWithURLByString:(NSString *)stringOfURL
                  completeBlock:(void (^)(NSDictionary *data)) parseCompleted dictionaryCloseInArray:(BOOL)close;
/* parsing xml by url string
 *
 */
+ (void)xmlParseWithXMLParser:(NSXMLParser *) parser
                completeBlock:(void (^)(NSDictionary *data)) parseCompleted;
+ (void)xmlParseWithXMLParser:(NSXMLParser *) parser
                completeBlock:(void (^)(NSDictionary *data)) parseCompleted
                   dictionaryCloseInArray:(BOOL) close;
/* parsing xml by NSXMLparser
 *
 */
@end

```

**XMLParser.m**

```{bash}

//
//  XMLParser.m
//  Test
//
//  Created by wuyingqiang on 7/19/15.
//  Copyright (c) 2015 wuyingqiang. All rights reserved.
//

#import "XMLParser.h"

/*
 * define the parsing mode
 *
 */
typedef NS_ENUM(NSInteger, ParseMode) {
    ParseModeByData     = 1,
    ParseModeByString   = 2,
    ParseModeByURL      = 3,
    ParseModeByXMLParser= 4
};

typedef void (^parseDoneBlock)(NSDictionary *data);

@interface XMLParser() <NSXMLParserDelegate>

/* block , which is for complete operation */
@property (nonatomic, weak) parseDoneBlock parseCompleteNow;

/* data from source */
@property (nonatomic, strong) NSData *data;
/* url of network */
@property (nonatomic, strong) NSURL *Url;

/* xml operation stack */
@property (nonatomic, strong) NSMutableArray *elementStack;

/* character of finding */
@property (nonatomic, strong) NSMutableString *textGet;

/* xml which array closed */
@property (nonatomic) BOOL close;

- (void)setURL:(NSString *)urlString;
- (void)startWithParseMode:(ParseMode)mode comcompleteBlock:(void (^)(NSDictionary *))parseCompleted dataSource:(NSObject *)source arrayClosed:(BOOL) close;

@end
@implementation XMLParser

#pragma mark - Class Method.

/* class method in no dictionary close in array mode.
 *
 * SUCH AS:
 * <data>
 *  <current_condition>
 *      <cloudcover>16</cloudcover>
 *      <humidity>59</humidity>
 *      <observation_time>09:09 PM</observation_time>
 *      <precipMM>0.1</precipMM>
 *      <pressure>1010</pressure>
 *      <temp_C>10</temp_C>
 *      <temp_F>49</temp_F>
 *      <visibility>10</visibility>
 *      <weatherCode>113</weatherCode>
 *      <weatherDesc>
 *          <Value>Clear</Value>
 *      </weatherDesc>
 *      <weatherIconUrl>
 *          <value>
 *              http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0008_clear_sky_night.png
 *          </value>
 *      </weatherIconUrl>
 *      <winddir16Point>NW</winddir16Point>
 *      <winddirDegree>316</winddirDegree>
 *      <windspeedKmph>47</windspeedKmph>
 *      <windspeedMiles>29</windspeedMiles>
 *  </current_condition>
 *
 * ----------------------------------------------------------------------------------------------------------------
 *  After `DICTIONARY IN ARRAY` parsing:
 *   
 *   data = {
 *            "current_condition" = [
 *                                   {
 *                                     cloudcover = 16;
 *                                       humidity = 59;
 *                                     "observation_time" = "09:09 PM";
 *                                     precipMM = "0.1";
 *                                     pressure = 1010;
 *                                     "temp_C" = 10;
 *                                     "temp_F" = 49;
 *                                     visibility = 10;
 *                                     weatherCode = 113;
 *                                     weatherDesc = [
 *                                                      {
 *                                                          value = Clear;
 *                                                      }
 *                                     ];
 *                                     weatherIconUrl = [
 *                                                      {
 *                                                          value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0008_clear_sky_night.png";
 *                                                      }
 *                                     ];
 *                                     winddir16Point = NW;
 *                                     winddirDegree = 316;
 *                                     windspeedKmph = 47;
 *                                     windspeedMiles = 29;
 *                                   }
 *                                  ];
 *
 * ---------------------------------------------------------------------------------------------------------------
 *  Without `DICTIONARY IN ARRAY` parsing:
 *
 *
 *   data = {
 *            "current_condition" = {
 *                                     cloudcover = 16;
 *                                       humidity = 59;
 *                                     "observation_time" = "09:09 PM";
 *                                     precipMM = "0.1";
 *                                     pressure = 1010;
 *                                     "temp_C" = 10;
 *                                     "temp_F" = 49;
 *                                     visibility = 10;
 *                                     weatherCode = 113;
 *                                     weatherDesc = {
 *                                                          value = Clear;
 *                                     }
 *                                     weatherIconUrl = {
 *                                                          value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0008_clear_sky_night.png";
 *                                     };
 *                                     winddir16Point = NW;
 *                                     winddirDegree = 316;
 *                                     windspeedKmph = 47;
 *                                     windspeedMiles = 29;
 *                                   };
 *
 * ------------------------------------------------------------------------------------------------------------
 *
 */

// generation mode
+ (void)xmlParseByData:(NSData *)data
         completeBlock:(void (^)(NSDictionary *))parseCompleted {

    [[[XMLParser alloc] init] startWithParseMode:ParseModeByData
                                comcompleteBlock:parseCompleted
                                      dataSource:data
                                     arrayClosed:NO];
}
+ (void)xmlParseByURL:(NSURL *)url
        completeBlock:(void (^)(NSDictionary *))parseCompleted {

    [[[XMLParser alloc] init] startWithParseMode:ParseModeByURL
                                comcompleteBlock:parseCompleted
                                      dataSource:url
                                     arrayClosed:NO];
}
+ (void)xmlParseWithURLByString:(NSString *)stringOfURL
                  completeBlock:(void (^)(NSDictionary *))parseCompleted {

    NSURL *url = [NSURL URLWithString:stringOfURL];

    [XMLParser xmlParseByURL:url
               completeBlock:parseCompleted];

}
+ (void)xmlParseWithXMLParser:(NSXMLParser *)parser
                completeBlock:(void (^)(NSDictionary *))parseCompleted {
    [[[XMLParser alloc] init] startWithParseMode:ParseModeByXMLParser
                                comcompleteBlock:parseCompleted
                                      dataSource:parser
                                     arrayClosed:NO];
}

// `dictionary in array` mode
+ (void)xmlParseByData:(NSData *)data completeBlock:(void (^)(NSDictionary *))parseCompleted dictionaryCloseInArray:(BOOL)close {
    [[[XMLParser alloc] init] startWithParseMode:ParseModeByData
                                comcompleteBlock:parseCompleted
                                      dataSource:data
                                     arrayClosed:close];
}
+ (void)xmlParseByURL:(NSURL *)url completeBlock:(void (^)(NSDictionary *))parseCompleted dictionaryCloseInArray:(BOOL)close {

    [[[XMLParser alloc] init] startWithParseMode:ParseModeByURL
                                comcompleteBlock:parseCompleted
                                      dataSource:url
                                     arrayClosed:close];
}
+ (void)xmlParseWithURLByString:(NSString *)stringOfURL completeBlock:(void (^)(NSDictionary *))parseCompleted dictionaryCloseInArray:(BOOL)close {

    NSURL *url = [NSURL URLWithString:stringOfURL];

    [XMLParser xmlParseByURL:url
               completeBlock:parseCompleted dictionaryCloseInArray:close];

}
+ (void)xmlParseWithXMLParser:(NSXMLParser *)parser
                completeBlock:(void (^)(NSDictionary *))parseCompleted
                   dictionaryCloseInArray:(BOOL)close {

    [[[XMLParser alloc] init] startWithParseMode:ParseModeByXMLParser
                                comcompleteBlock:parseCompleted
                                      dataSource:parser
                                     arrayClosed:close];
}

#pragma mark - Private Method.
- (void)setURL:(NSString *)urlString
/*
 * set the URL
 *
 */
{

    [self setUrl:[NSURL URLWithString:urlString]];
}

- (void)startWithParseMode:(ParseMode)mode comcompleteBlock:(void (^)(NSDictionary *))parseCompleted dataSource:(NSObject *)source arrayClosed:(BOOL) close
/*
 * start to parsing ...
 *
 */
{


    NSXMLParser *parse;
    _close = close;

    /* block */
    _parseCompleteNow = parseCompleted;

    /* set the parse mode */
    switch (mode) {
        case ParseModeByData:
        {
            NSData *data = (NSData *)source;
            parse = [[NSXMLParser alloc] initWithData:data];
        }
            break;
        case ParseModeByURL:
        {
            NSURL *url = (NSURL *)source;
            parse = [[NSXMLParser alloc] initWithContentsOfURL:url];

        }
        case ParseModeByXMLParser: {

            parse = (NSXMLParser *)source;
        }
            break;
        default:
            break;
    }

    [self startWithParseInstance:parse];

}
- (void)startWithParseInstance:(NSXMLParser *)parse {

    parse.delegate = self;
    [parse parse];
}


#pragma mark - XML parse Delegate
- (void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName attributes:(NSDictionary *)attributeDict {

    /* get the parent dictionary */
    NSMutableDictionary *parentDict = [_elementStack lastObject];
    NSObject *elementFound = [parentDict objectForKey:elementName];

    /* if not exist */
    if(!elementFound) {

        NSMutableDictionary *childDict = [NSMutableDictionary new];
        [parentDict setObject:childDict forKey:elementName];

        [_elementStack addObject:childDict];

    } else  {/* exist this key, means this is array, not dictionary */

        /* start from array */
        if( [elementFound isKindOfClass:[NSMutableDictionary class]]) {

            NSMutableArray *childs = [NSMutableArray new];
            [childs addObject:elementFound];

            [parentDict setObject:childs forKey:elementName];

            [childs addObject:[NSMutableDictionary new]];
            [_elementStack addObject:[childs lastObject]];
        }

        else {

            /* continue from array */
            NSMutableDictionary *childDict = [NSMutableDictionary new];
            NSMutableArray *items = (NSMutableArray *)elementFound;

            [items addObject:childDict];

            [_elementStack addObject:childDict];

        }
    }

}
- (void)parser:(NSXMLParser *)parser didEndElement:(NSString *)elementName namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName{

    /* has found the text */
    if( [self.textGet length] > 0) {

        [_elementStack removeLastObject];
        NSMutableDictionary *item = [_elementStack lastObject];

        [item setObject:_textGet forKey:elementName];
        _textGet = [NSMutableString new];

    } else {

        [_elementStack removeLastObject];

        if (_close) { // when open the <close in Array> mode

            id value = [[_elementStack lastObject] valueForKey:elementName];
            if ([value isKindOfClass:[NSDictionary class]]) {

                NSMutableArray *arrayClose = [NSMutableArray array];
                [arrayClose addObject:value];

                [[_elementStack lastObject] setValue:arrayClose forKey:elementName];
            }
        }


    }




}
- (void)parser:(NSXMLParser *)parser foundCharacters:(NSString *)string {

    if([string hasPrefix:@"\n"])
        return ;

    [_textGet appendString:string];
}

- (void)parser:(NSXMLParser *)parser parseErrorOccurred:(NSError *)parseError {

    /* something error */
    NSLog(@"%@", parseError);
}
- (void)parserDidStartDocument:(NSXMLParser *)parser
/* start to parse */
{

    _elementStack = [NSMutableArray new];
    _textGet = [NSMutableString new];

    [_elementStack addObject:[NSMutableDictionary new]];
}

- (void)parserDidEndDocument:(NSXMLParser *)parser {
    /* parse done */
    NSLog(@"fater parse:\n %@", [_elementStack lastObject]);

    NSDictionary *compeltedData = [_elementStack lastObject];

    /* second dealing */
    compeltedData = [self settingArrayClosed:compeltedData];

    self.parseCompleteNow(compeltedData);
}

#pragma mark - Class Dead
- (void)dealloc {
    NSLog(@"xml class dead");
}

#pragma mark - setting array closed
- (NSDictionary *)settingArrayClosed:(NSDictionary *)result {

    /* first, find the main key */
    NSString *key = [result allKeys][0];

    /* second, start to close */
    NSDictionary *start = @{ key : [result valueForKey:key][0] };

    return start;
}

@end

```


## 预期的结果

```{bash}

data =     (
                {
            "current_condition" =             (
                                {
                    cloudcover = 16;
                    humidity = 59;
                    "observation_time" = "09:09 PM";
                    precipMM = "0.1";
                    pressure = 1010;
                    "temp_C" = 10;
                    "temp_F" = 49;
                    visibility = 10;
                    weatherCode = 113;
                    weatherDesc =                     (
                                                {
                            value = Clear;
                        }
                    );
                    weatherIconUrl =                     (
                                                {
                            value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0008_clear_sky_night.png";
                        }
                    );
                    winddir16Point = NW;
                    winddirDegree = 316;
                    windspeedKmph = 47;
                    windspeedMiles = 29;
                }
            );
            request =             (
                                {
                    query = "Lat 32.35 and Lon 141.43";
                    type = LatLon;
                }
)
            );
            weather =             (
                                {
                    date = "2013-01-15";
                    precipMM = "1.8";
                    tempMaxC = 12;
                    tempMaxF = 53;
                    tempMinC = 10;
                    tempMinF = 50;
                    weatherCode = 119;
                    weatherDesc =                     (
                                                {
                            value = Cloudy;
                        }
                    );
                    weatherIconUrl =                     (
                                                {
                            value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0003_white_cloud.png";
                        }
                    );
                    winddir16Point = NNW;
                    winddirDegree = 348;
                    winddirection = NNW;
                    windspeedKmph = 66;
                    windspeedMiles = 41;
                },
                                {
                    date = "2013-01-16";
                    precipMM = "0.6";
                    tempMaxC = 13;
                    tempMaxF = 56;
                    tempMinC = 11;
                    tempMinF = 51;
                    weatherCode = 113;
                    weatherDesc =                     (
                                                {
                            value = Sunny;
                        }
                    );
                    weatherIconUrl =                     (
                                                {
                            value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0001_sunny.png";
                        }
                    );
                    winddir16Point = WNW;
                    winddirDegree = 284;
                    winddirection = WNW;
                    windspeedKmph = 33;
                    windspeedMiles = 21;
                },
                                {
                    date = "2013-01-17";
                    precipMM = "0.5";
                    tempMaxC = 14;
                    tempMaxF = 56;
                    tempMinC = 7;
                    tempMinF = 44;
                    weatherCode = 119;
                    weatherDesc =                     (
                                                {
                            value = Cloudy;
                        }
                    );
                    weatherIconUrl =                     (
                                                {
                            value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0003_white_cloud.png";
                        }
                    );
                    winddir16Point = WNW;
                    winddirDegree = 293;
                    winddirection = WNW;
                    windspeedKmph = 41;
                    windspeedMiles = 25;
                },
                                {
                    date = "2013-01-18";
                    precipMM = "1.9";
                    tempMaxC = 11;
                    tempMaxF = 51;
                    tempMinC = 7;
                    tempMinF = 44;
                    weatherCode = 353;
                    weatherDesc =                     (
                                                {
                            value = "Light rain shower";
                        }
                    );
                    weatherIconUrl =                     (
                                                {
                            value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0009_light_rain_showers.png";
                        }
                    );
                    winddir16Point = NW;
                    winddirDegree = 312;
                    winddirection = NW;
                    windspeedKmph = 66;
                    windspeedMiles = 41;
                },
                                {
                    date = "2013-01-19";
                    precipMM = "1.1";
                    tempMaxC = 7;
                    tempMaxF = 45;
                    tempMinC = 6;
                    tempMinF = 43;
                    weatherCode = 176;
                    weatherDesc =                     (
                                                {
                            value = "Patchy rain nearby";
                        }
                    );
                    weatherIconUrl =                     (
                                                {
                            value = "http://www.worldweatheronline.com/images/wsymbols01_png_64/wsymbol_0009_light_rain_showers.png";
                        }
                    );
                    winddir16Point = NW;
                    winddirDegree = 324;
                    winddirection = NW;
                    windspeedKmph = 52;
                    windspeedMiles = 32;
                }
            );
        }
    );
}
)

```
