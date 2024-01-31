---
title: "Gradle初探"
date: 2023-12-26T14:14:31+08:00
author: "celine"
tags: [""]
categories: ["StudyNote"]
---

1. ### 概觀

Gradle是一個基於Groovy的建構管理系統，它特別設計用來建構基於Java的專案

Gradle運行在所有主流作業系統，只需要Java JDK 8 或者以上的版本即可安裝

(1) Unix的作業系統，可以用 SDKMAN package manager 安裝，指令`sdk install gradle 8.5`

​    macOS的作業系統，可以用Homebrew package manager安裝，指令 `brew install gradle`

(2) 設置環境

- Linux or MacOS: 在所選的目錄下解壓縮

  ```terminal
  $ mkdir /opt/gradle
  $ unzip -d /opt/gradle gradle-8.5-bin.zip
  $ ls /opt/gradle/gradle-8.5
  LICENSE NOTICE bin getting-started.html init.d lib media
  ```

- Microsoft Windows

  用 File Explorer 創建新的目錄 `C:\Gradle`，再把下載後解壓縮的 `gradle-8.5` 資料夾拉進去這個目錄

(3) 配置環境變數

- Linux or MacOS: 配置 `PATH` 環境變數，注意要包括解壓縮後擋彈的 `bin` 目錄

  ```terminal
  $ export PATH=$PATH:/opt/gradle/gradle-8.6/bin
  ```

- Microsoft Windows: 
  [This PC/Computer] 右鍵 → Properties → Advanced System Settings → Environment Variable
  在`System Variables`，選擇`Path`→ `Edit`，加上 `C:\Gradle\gradle-8.5\bin` 接著存擋

(4) 驗證安裝是否成功：`gradle -v`

2. ### 建構的基石：Projects, Tasks

Gradle Build 包含了一至多個 projects，每一個 project 含有一到多個 tasks

Gradle 中的 project 可以組成一個 jar檔, war檔, 甚至是 zip檔

以下是一個簡易的 task，如果在 `build.gradle` 這檔案的同一層目錄下，執行 `gradle -q hello`，可以在 console 看到 output

```groovy
task hello {
  doLast {
    println 'Happy New Year'
  }
}
```

2. ### 1 Tasks

Gradle 的 build scripts 就是 Groovy

```groovy
task toLower {
  doLast {
    String someString = 'HELLO FROM BAELDUNG'
    println "Original: "+ someString
    println "Lower case: "+ someString.toLowerCase()
  }
}
```

---

## 比較 Ant, Maven, Gradle

Ant 老 Maven 中 Gradle 青

#### Apache Ant

Apache Ant (Another Neat Tool) 是一個自動化 bulid process 的 Java library。

此外，Ant 也可以用來建構非 Java 的 app。他原先是Apache Tomcat代碼庫的一部分，在2000年發佈為一個獨立的專案。

Ant build 檔用 XML 寫成的，約定俗成的命名是 `build.xml`，主要會有\<project>、\<target> 標籤。要編譯code時，在終端機輸入 `ant compile` 就會觸發底下所有的 target。

但是因為 Ant 沒有內建依賴管理的支援，維護XML build 檔也相當困難，所以出現了Maven取代之。

#### Apache Maven

[Maven](https://maven.apache.org/) 是一個依賴管理與自動化建構的工具，主要用在Java應用程式。Maven也是用 xml檔，但是管理方式比較易於管理，重點在於約定大於配置 (convention over configuration)。

優點：Maven著重於 build 要做的事情並提供框架，另外，Maven也提供內建的依賴管理支援

Maven的配置檔 `pom.xml`，包含了build 與依賴管理的指示

與Ant的差別在於，不需要在手動定義 build process 的每個階段(phase)，直接call Maven內建指令。例如：編譯程式的話，敲這個指令`mvn compile`

Maven可以視為一個插件執行框架，所有作業都藉由plugin來執行。其中一個plugs `Apache Maven Dependency Plugin` 有一個 *copy-dependencies goal*，會將dependencies 複製到一個特定目錄下。

實際例子：

以下的pom.xml檔案，會在 <u>**package**</u> 階段執行 copy-dependencies 插件

```xml
<!--pom.xml-->
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-dependency-plugin</artifactId>
            <executions>
                <execution>
                    <id>copy-dependencies</id>
                    <phase>package</phase>
                    <goals>
                        <goal>copy-dependencies</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>target/dependencies
                          </outputDirectory>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

輸入以下指令的話，就會執行這段 plugin 並將依賴檔案們複製到 `target/dependencies` 資料夾下。

```terminal
mvn package
```

雖然 Maven 配置檔比 Ant files 更為標準化，但它的strict conventions也使其靈活性遠低於 Ant，撰寫客製化的 build script 比起 Ant 困難多了。因此出現了 Gradle，集成了Ant的靈活性與Maven的特色。

#### Gradle

Gradle是基於Ant與Maven概念之上的依賴管理與自動化建構的工具。不用 XML 格式寫配置了！

開發傾向於使用領域特定語言(domain-specific language)解決問題，而Gradle使用的就是DSL（Groovy或者Kotlin）。因為該DSL是特別設計來解決特定領域問題的，所以配置檔案會比較小也比較不凌亂。用Groovy寫成的Gradle配置檔命名為 `build.gradle`，而用Kotlin寫成的配置檔則命名為 `build.gradle.kts`。

相較於Groovy，Kotlin提供較佳的 IDE 支援（包含程式碼錯誤偵測以及自動完成）。

執行編譯程式的指令

```terminal
gradle classes
```

示例代碼（build.gradle）

```Groovy
apply plugin: 'java'

repositories {
    mavenCentral()
}

jar {
    baseName = 'gradleExample'
    version = '0.0.1-SNAPSHOT'
}

dependencies {
    testImplementation 'junit:junit:4.12'
}
```

關於build steps建構步驟的命名：Ant之於targets，Maven之於phases，Gradle之於tasks

Gradle 複製依賴的程式如下

```groovy
task copyDependencies(type: Copy) {
  from configurations.compile
  into 'dependencies'
}
```

若要執行上述task，可以輸入

```terminal
gradle copyDependencies
```

#### 結論

目前build tool市場上，Maven仍屬大宗，但Gradle很適合應用在複雜的代碼庫，原因如下：

1. 很多開源專案（例如Spring）都在使用
2. 大部分情況下都比Maven還要快，有鑒於其 incremental build 特性
3. 提供先進的分析與除錯服務 ([Build Scan](https://scans.gradle.com/))

但如果你不熟Groovy或Kotlin的話，學習曲線就比較高

補充：Gradle Wrapper 是會調用一個有宣告版本之Gradle 的腳本

#### 參考資料

[Introduction to Gradle](https://www.baeldung.com/gradle)

[Ant vs Maven vs Gradle](https://www.baeldung.com/ant-maven-gradle)

[如何用Maven建立可執行JAR檔](https://www.baeldung.com/executable-jar-with-maven)

#### 實作例子

[使用Gradle建構Java應用程式_baeldung](https://www.baeldung.com/gradle-building-a-java-app)

[使用Gradle建構Java應用程式_spring.io](https://spring.io/guides/gs/gradle/)
