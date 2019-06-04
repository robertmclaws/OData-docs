---
title: "Parsing uri path template"
description: ""
author: saumadan
ms.author: saumadan
ms.date: 02/19/2019
ms.topic: article
ms.service: multiple
---
# Parsing uri path template

From ODataLib 6.11.0, OData uri parser can parse uri path template. A path template is any identifier string enclosed with curly braces.
For example: 
```C#
{dynamicProperty}
```

# Uri templates

There are three kinds of template:

1. Key template:  ~/Customers({key})
2. Function parameter template: ~/Customers/Default.MyFunction(name={name})
3. Path template: ~/Customers/{dynamicProperty}

Be caution:

1. In UriParser instance, please set EnableUriTemplateParsing = true.
2. Path template can't be the first segment.

# Example

```C#
var uriParser = new ODataUriParser(HardCodedTestModel.TestModel, new Uri("People({1})/{some}", UriKind.Relative))  
{  
  EnableUriTemplateParsing = true  
};

var paths = uriParser.ParsePath().ToList();

var keySegment = paths[1].As<KeySegment>();
var templateSegment = paths[2].As<PathTemplateSegment>();
templateSegment.LiteralText.Should().Be("{some}"); 

```
