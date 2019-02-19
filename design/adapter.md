# 3.8：适配器模式

想使用一个已经存在的类，但他的接口不符合需求。要让两个不能通讯的类通讯，让类功能扩展

对象适配器比较通用



### 1: 背景

```
一个来自中国的外交官参加联合国大会，但是他不会说其他语言，好在他带了翻译器
```



### 1: 适配器模式的做法

```java
/**
 * 一个实实在在的中国人,自己就可以说中文
 */
public class ChinesePerson {
    private String name;
    public void sayChinese() {
        System.out.println(name + " 可以说中文");
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}


/**
 * 翻译器
 */
public interface Translator {

    void sayEnglish(String name);

    void sayJapanese(String name);
}

/**
 * 翻译器的实现类
 */
public class TranslatorImpl implements Translator {
    @Override
    public void sayEnglish(String name) {
        System.out.println(name + " 通过翻译器说英语");
    }

    @Override
    public void sayJapanese(String name) {
        System.out.println(name + " 通过翻译器说日语");
    }
}

/**
 * 适配器类，通过此类，该同志获得了更多的语言能力
 */
public class ChinesePersonAdapter {

    private ChinesePerson chinesePerson;
    private Translator translatorImpl;

    public ChinesePersonAdapter(ChinesePerson chinesePerson, Translator translatorImpl) {
        this.chinesePerson = chinesePerson;
        this.translatorImpl = translatorImpl;
    }

    public void sayChinese() {
        chinesePerson.sayChinese();
    }

    public void sayEnglish() {
        translatorImpl.sayEnglish(chinesePerson.getName());
    }

    public void sayJapanese() {
        translatorImpl.sayJapanese(chinesePerson.getName());
    }
}

public class Client {

    public static void main(String[] args) {

        ChinesePerson chinesePerson = new ChinesePerson();
        chinesePerson.setName("张三");

        Translator translator = new TranslatorImpl();

        ChinesePersonAdapter chinesePersonAdapter = 
                new ChinesePersonAdapter(chinesePerson, translator);

        chinesePersonAdapter.sayChinese();
        chinesePersonAdapter.sayEnglish();
        chinesePersonAdapter.sayJapanese();

    }
}

张三 可以说中文
张三 通过翻译器说英语
张三 通过翻译器说日语
```

```
通过适配器的适配，该同志具备了说多国语言的能力，即不改变这个人本身，也不该变翻译器，而是把2者组合使用
```



