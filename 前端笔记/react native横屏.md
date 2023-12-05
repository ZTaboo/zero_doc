## 如何强制横屏模式

修改安卓目录下 `app/src/main/AndroidManifest.xmlactivity` 标签增加`android:screenOrientation="landscape"`

## 隐藏顶部信息栏

添加`StatusBar` 组件

```react
import {
  View,
  StatusBar,
} from 'react-native';

const App: () => react.Node = () => {
  return (
    <View>
      <StatusBar
        hidden={true}
      />
    </View>
  );
};
export default App;
```

## 隐藏底部导航

修改：`android\app\src\main\java\com\learn\MainActivity.java` 文件，在`MainActivity`类中新增如下方法

```java
import android.os.Bundle;
import android.view.View;

@Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    hideNavigationBar();
  }

  @Override
  public void onWindowFocusChanged(boolean hasFocus) {
    super.onWindowFocusChanged(hasFocus);
    if (hasFocus) {
      hideNavigationBar();
    }
  }

  private void hideNavigationBar() {
    getWindow().getDecorView().setSystemUiVisibility(
            View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
                    | View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY);

  }
```

## 获取屏幕宽高

```react
import {
  Dimensions,
} from 'react-native';

const windowWidth = Dimensions.get('window').width;
  const windowHeight = Dimensions.get('window').height;
  useEffect(() => {
    console.log(`width :${windowWidth},height: ${windowHeight}`);
  }, []);
```
