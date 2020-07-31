### fat-aar打包工具使用

**PS: Gradle相关推荐使用下面版本，其他版本可能出现未知错误**
- Gradle Plugin版本3.4.1
- Gradle版本5.1.1
***

1.项目引用插件
- 项目根目录build.gradle
  ```groovy
  buildscript {
      repositories {
          maven { url './repo' }
          ...
      }
      dependencies {
          ...
          classpath "com.kezong:fat-aar:1.2.10" //fat-aar插件
      }
  }
  ```

2.合并打包模块build.gradle
- 使用插件`apply plugin: 'com.kezong.fat-aar'`
- 使用`ndk abiFilters`限制so CPU架构
  ```groovy    
  android {
        ...
        defaultConfig {
            ...
            ndk {
                abiFilters 'armeabi-v7a'
                abiFilters 'arm64-v8a'
            }
        }
    }
  ```
- 需要合并打包aar的模块
  ```groovy
  dependencies {
      implementation fileTree(dir: 'libs', include: ['*.jar'])
      embed project(path: ':test', configuration:'default')
      ...
  }
  ```
  
3.合并打包有相同资源文件问题打包失败
> `gradle.properties`下加上一行`android.disableResourceValidation=true`
    