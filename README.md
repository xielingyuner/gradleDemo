# gradleDemo

```groovy
// 项目依赖插件
//  plugins 是应用插件的新方法，它们必须在Gradle插件库中可用。apply 是一种较老的、但更灵活的向构建中添加插件的方法。
// 新的plugins方法不能在多项目配置中工作(子项目，所有项目)，但可以在每个子项目的构建配置中工作。
plugins {
id 'org.springframework.boot' version '2.6.3'
id 'io.spring.dependency-management' version '1.0.11.RELEASE'
id 'java'
}
//  项目组
group = 'com.example'
//  项目版本号
version = '0.0.1-SNAPSHOT'
//  对应的JVM版本
sourceCompatibility = '11'

// 使用maven做为jar包的信赖管理，通过maven仓库下载项目所需的信赖包
apply plugin: 'maven'
// 指定项目为java项目，项目编译(在项目提示符下执行：gradle build)时生成项目的jar包。
apply plugin: 'java'
// IntelliJ IDEA 插件
apply plugin: 'idea'
// war包插件:指定web项目，项目编译(在项目提示符下执行：gradle build)时生成项目的war包。
apply plugin: 'war'
// 插件将构建web项目的开发环境，生成所需要的.project,.classpath等文件。因为我web开发使用的是eclipse-j2ee版本，所以指定为wtp环境。
apply plugin: 'eclipse-wtp'
// 加入jetty的支持，代码修改后直接执行命令gradle jettyRun即可运行web项目。
apply plugin: 'jetty'

//  指定本地仓 远程仓
repositories {
//    指定本地仓, 引用本地仓库
    mavenLocal()
//    使用阿里巴巴的maven仓库
    maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
//    使用开源中国的maven仓库
    maven { url 'http://maven.oschina.net/content/groups/public/' }
//    告诉Gradle从Maven中央仓库获取工具库的内容 / https://repo.maven.apache.org/maven2/
    mavenCentral()
}

// 该配置会被应用到所有的子工程。
allprojects {
    repositories {
        mavenLocal()
    }
}

dependencies {

//  compile
// 导入本地的依赖包
    compile fileTree(dir: 'libs', include: ['*.jar'])
// 从maven仓库导入依赖包
    compile('org.apache.httpcomponents:httpclient:4.4')
    compile 'org.apache.httpcomponents:httpclient:4.4'
    compile group: 'com.jfinal', name: 'jfinal', version: '3.0'

    // 也可以集合到一起写
    compile(
            "dir: 'libs', include: ['*.jar']",
            "org.apache.httpcomponents:httpclient:4.4",
            "org.apache.commons:commons-lang3:3.3.2"
    )


//  compileConly
//  runtimeOnly
//  implementation

//  testCompileOnly
//  testImplementation
//  testAnnotationProcessor
//  testRuntimeOnly

//  jasper
//  antContrib
//  externalLibs
//  deploymentTools
//  js
//  androidTestCompile
//  api
//  annotationProcessor
//  provided
//  apk
//  providedCompile
//  AndroidTestImplementation
//  debugApi

    compileOnly 'org.springframework.boot:spring-boot-starter'
    implementation 'org.springframework.boot:spring-boot-starter'
    testCompileOnly 'org.springframework.boot:spring-boot-starter'
    testImplementation 'org.springframework.boot:spring-boot-starter'
    testAnnotationProcessor 'org.springframework.boot:spring-boot-starter'
    jasper 'org.springframework.boot:spring-boot-starter'
}

//循环遍历
task foreach << {
    10.times { println "the times is $it." }
}

依赖其他任务
task intro(dependsOn: hello) << {
    println "I'm Gradle---intro"
}

//动态任务
4.times { counter ->
    task "task$counter" << {
        println "I'm task number $counter"
    }
}
// gradle task[0-4]
// 循环四次，生成4个task， 任务名分别为task0,task1,task2,task3

// 使用外部任务的属性
task myTask {
    ext.myProperty = "myValue"
}
task extraProps << {
    println myTask.myProperty
}

//  使用ant任务
task loadfile1 << {
    def files = file('../builds').listFiles().sort()
    files.each { File file ->
        if (file.isFile()) {
            ant.loadfile(srcFile: file, property: file.name)
            println " *** $file.name ***"
            println "${ant.properties[file.name]}"
        }
    }
}

//可以使用build.gradle文件中的以下代码段对测试方法进行分组。
test {
    useJUnit {
        includeCategories 'org.gradle.junit.CategoryA'
        excludeCategories 'org.gradle.junit.CategoryB'
    }
}



tasks.named('test') {
    useJUnitPlatform()
}
```


