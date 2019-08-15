# Android Studio下打包aidl到jar包（图解）

##### 新建module，则build.gradle首行为`apply plugin: 'com.android.library'`

![](https://upload-images.jianshu.io/upload_images/9601136-a381756d35ea4c65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

##### 添加aidl文件

![](https://upload-images.jianshu.io/upload_images/9601136-230628ac19eb9cde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 在build.gradle末尾添加如下代码

```
// Automatic calculation of version number based date: 2018-11-16 <==> 2.4.0 (240)
// Format: (Year - 2016).(Month).(Day)
TimeZone.setDefault(TimeZone.getTimeZone("Asia/Shanghai"))
def date = new Date()
def year = date[Calendar.YEAR]
def month = 1 + date[Calendar.MONTH]        // 0 ~ 11
def day = date[Calendar.DAY_OF_MONTH]
//versionCode Integer.parseInt(String.format("%d%02d%02d", year - 2016, month, day))
def versionName = String.format("%02d%02d", month, day)

// 修改jar名称 将指定的jar生成的地方
task makeJar(type: Copy) {
    //删除存在的
//    delete 'hsaemusic' + versionName + '.jar'
    //设置拷贝的文件
    from('build/intermediates/packaged-classes/release/')
    //打进jar包后的文件目录
    into('libs/')
    // 将class.jar放入build/libs/目录下
    // include , exclude 参数来设置过滤
    // 我们只关心 classes.jar 这个文件
    include('classes.jar')
    //重命名
    rename('classes.jar', 'hsaemusic' + versionName + '.jar')
}
makeJar.dependsOn(build)
```

##### 在Android Studio窗口右上点击`Gradle`，双击`other`下的`makeJar`
![](https://upload-images.jianshu.io/upload_images/9601136-aae77b2870c417fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/9601136-011a4bfd4fae2e5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 生成的jar包如下所示，包含aidl对应的.class字节码文件

![](https://upload-images.jianshu.io/upload_images/9601136-674afdfafc83de22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

步骤已截图，代码非截图，可直接copy 
[简书地址](https://www.jianshu.com/p/c019b7a7e71f)  
欢迎点赞，万分感谢



