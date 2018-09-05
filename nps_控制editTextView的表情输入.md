### 一、对EditTextView的输入做表情限制处理（不允许输入表情）

1、EditTextView允许输入表情，但是提交的时候检测输入的内容是否有表情，检测方法如下：

```java
private static final Pattern emoji = Pattern.compile(
            "[\ud83c\udc00-\ud83c\udfff]|[\ud83d\udc00-\ud83d\udfff]|[\u2600-\u27ff]",
            Pattern.UNICODE_CASE | Pattern.CASE_INSENSITIVE);

public static boolean checkHasEmoji(CharSequence charSequence) {
        if (charSequence == null) return false;
        Matcher matcher = emoji.matcher(charSequence);
        if (matcher.find()) return true;
        else return false;
    }
```

2、对EditText通过设置表情的过滤，在输入表情的时候无效：

```java
private static InputFilter[] emojiFilters = {new EmojiFilter()};
editText.setFilters(emojiFilters)
//EmojiFilter主要是表情的过滤器
EmojiFilter()
```

3、注意在限制了输入表情的filter之后，有可能在代码中设置的字数就无效了，主要原因是filter中有对于输入框字数限制的方法，若未重写，则不会生效。

```java
InputFilter emojiFilter = new EmojiFilter();
discussQuestionInput.setFilters(new InputFilter[]{
    new InputFilter.LengthFilter(2000), emojiFilter
});
```

