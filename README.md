# XposedDemo

[lpcdma's Blog](http://lpcdma.com)

![](https://raw.githubusercontent.com/lpcdma/XposedDemo/master/app/src/main/res/mipmap-xxxhdpi/ic_launcher.png)

- 1.方便使用android studio进行xposed开发
- 2.lib目录和libs目录**provided**,*compile*;暂且理解前者不会被编译到包中。

> 细节
>> 很重要

--------------------------------------------------

## 表格

dog | bird | cat
----|------|----
foo | foo  | foo
bar | bar  | bar
baz | baz  | baz
--------------------------------------------------

## 代码

```
public class RedClock implements IXposedHookLoadPackage {
    public void handleLoadPackage(final LoadPackageParam lpparam) throws Throwable {
        if (!lpparam.packageName.equals("com.android.systemui"))
            return;

        findAndHookMethod(Application.class, "attach", Context.class, new XC_MethodHook() {
            @Override
            protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                findAndHookMethod("com.android.systemui.statusbar.policy.Clock", lpparam.classLoader, "updateClock", new XC_MethodHook() {
                    @Override
                    protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                        TextView tv = (TextView) param.thisObject;
                        String text = tv.getText().toString();
                        tv.setText(text + " :)");
                        tv.setTextColor(Color.RED);
                    }
                });
            }
        });
    }
}
```
