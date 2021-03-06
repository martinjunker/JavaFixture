[![Build Status](https://travis-ci.org/Nylle/JavaFixture.svg?branch=master)](https://travis-ci.org/Nylle/JavaFixture)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.github.nylle/javafixture/badge.svg)](http://search.maven.org/#artifactdetails|com.github.nylle|javafixture|0.1.2|)

# JavaFixture
JavaFixture is the attempt to bring the incredibly easy usage of [Mark Seemann's AutoFixture for .NET](https://github.com/AutoFixture/AutoFixture) to the Java world using [Mahmoud Ben Hassine's Random Beans](https://github.com/benas/random-beans).

The purpose of both projects is to generate full object graphs for use in test suites. For additional details refer to the respective project pages.

JavaFixture is simply wrapping [Random Beans](https://github.com/benas/random-beans) to provide a fluent API to customise your test objects during generation.

# Getting Started
```xml
<dependency>
    <groupId>com.github.nylle</groupId>
    <artifactId>javafixture</artifactId>
    <version>0.1.2</version>
    <scope>test</scope>
</dependency>
```
# Usage

## Create a Fixture
```java
JavaFixture fixture = new JavaFixture();
```

## Autogenerated String
```java
String result = fixture.create(String.class);
```
### Sample Result
String: "GPnGAwkxdahMweXqMgiqAuHjhpdqyqU"

## Autogenerated Number
```java
int result = fixture.create(int.class);
```
### Sample Result
int: -1612385443

## Complex Type
```java
ParentDto result = fixture.create(ParentDto.class);
```
### Sample Result
- ParentDto:
    - id: String: "ZsiFwiJrNVxAxdsCXpKcCizlnPhwX"
    - child: ChildDto:
        - id: String: "AwkxdahMweGPnGXqMgiqAuHjhpdqyqU"
        - names: ArrayList:
            - String: "livkzLQQghkBpMGNOdUoqhVAGUirJ"
            - String: "KNbEismpNwD"
            - String: "qgdblIauhjsNZderSCjMHxqVjhgt"

## Collection of Strings
```java
List<String> result = fixture.createMany(String.class).collect(Collectors.toList());
```
### Sample Result
ArrayList: 
- String: "ivscsrjNau"
- String: "oKEPtq"
- String: "ZOwobDdjXaqeaPeKWZviBeLyNhP"

## Add to Collection
```java
List<String> result = new ArrayList<>();
result.add("HELLO!");
fixture.addManyTo(result, String.class);
```
### Sample Result
ArrayList: 
- String: "HELLO!"
- String: "ivscsrjNau"
- String: "oKEPtq"
- String: "ZOwobDdjXaqeaPeKWZviBeLyNhP"

## Set Public Field
```java
TestDto result = fixture.build(TestDto.class)
                        .with(x -> x.myPublicField = 123)
                        .create();
```
### Sample Result
TestDto:
- myPrivateField: String: "jATiorjQTFkuVRIZTGPfqqdqEjLpld"
- myPublicField: int: 123

## Set Private Field
```java
TestDto result = fixture.build(TestDto.class)
                        .with("myPrivateField", "HELLO!")
                        .create();
```
### Sample Result
TestDto:
- myPrivateField: String: "HELLO!"
- myPublicField: int: 26123854

## Call a Setter
```java
TestDto result = fixture.build(TestDto.class)
                        .with(x -> x.SetMyPrivateField("HELLO!"))
                        .create();
```
### Sample Result
TestDto:
- myPrivateField: String: "HELLO!"
- myPublicField: int: 71

## Omit Field
```java
TestDto result = fixture.build(TestDto.class)
                        .without("myPrivateField")
                        .create();
```
### Sample Result
TestDto:
- myPrivateField: String: null
- myPublicField: int: -128564

## Omit Primitive Field (will initialize with default value)
```java
TestDto result = fixture.build(TestDto.class)
                        .without("myPublicField")
                        .create();
```
### Sample Result
TestDto:
- myPrivateField: String: "WXQpGbcJisFXveSyNATFbNYrjdKsqu"
- myPublicField: int: 0


## Perform Multiple Operations
```java
String child = fixture.create(String.class);
ParentDto parent = fixture.build(ParentDto.class)
                          .with(x -> x.addChild(child))
                          .with(x -> x.youngestChild = child)
                          .create();
```
### Sample Result
ParentDto:
- children: ArrayList:
    - String: "KAzec"
    - String: "EZoYxaAeGkpzaDMZ"
    - String: "FCMuhGbBvVuAKGLlgCyPv"
    - String: "rgmGQsaf"
- youngestChild: String: "rgmGQsaf"
