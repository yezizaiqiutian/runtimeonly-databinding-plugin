# runtimeonly-databinding-plugin
[![](https://jitpack.io/v/yezizaiqiutian/runtimeonly-databinding-plugin.svg)](https://jitpack.io/#yezizaiqiutian/runtimeonly-databinding-plugin)

## from:https://github.com/michaelrepo/DataBindingExtendPlugin

## 解决runtimeonly无法使用databinding问题,组件中DataBinderMapperImpl无法加入app主项目中

解决app使用runtimeOnly依赖组件后，组件的DataBinderMapperImpl不能被app依赖。 此插件修改class文件，将各组件的DataBinderMapperImpl插入到app的DataBinderMapperImpl的collectDependencies的list中。

注意：此项目的app主工程，不用来测试插件。如果需要测试插件，请自行修改app和插件的代码。


## 调用方式:
```
    //项目下gradle
    classpath 'com.github.yezizaiqiutian:runtimeonly-databinding-plugin:0.0.1'
    
    //app下gradle
    apply plugin: 'com.gh.databinding-ext'
  
    //以供DataBinding-Extend插件使用。
    //主功能的路径
    ext.mainProjectPath = "com/gh/ghandroidproject"
    //保存主工程包含的组件名(去掉了lib_前缀)，以供DataBinding-Extend插件使用
    //例:com/gh/main
    ext.mainIncludesPath = new ArrayList<String>()
    //记录哪些依赖，在打aar时不加入此依赖，需要从POM中移除
    ext.aarExcludes = new ArrayList<String>()

```

## 实现功能:
插入前

```
  @Override
  public List<DataBinderMapper> collectDependencies() {
    ArrayList<DataBinderMapper> result = new ArrayList<DataBinderMapper>(2);
    result.add(new androidx.databinding.library.baseAdapters.DataBinderMapperImpl());
    return result;
  }
```

插入后

```
  @Override
  public List<DataBinderMapper> collectDependencies() {
    ArrayList<DataBinderMapper> result = new ArrayList<DataBinderMapper>(2);
    result.add(new androidx.databinding.library.baseAdapters.DataBinderMapperImpl());
    result.add(new com.wawj.demo.DataBinderMapperImpl());
    return result;
  }
```


