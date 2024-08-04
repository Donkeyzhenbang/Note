## Cpp游戏开发

### 井字棋

要点：

1.利用类似消息队列机制拉取信息

2.及时清空/更新画面

3.双缓冲

```cpp
#include <graphics.h>
#include <iostream>

int main() {
	//printf("%ws", GetEasyXVer());
	//std::wcout << GetEasyXVer() << std::endl;
	initgraph(1200, 720);
	int x = 300;
	int y = 300;
	BeginBatchDraw();

	while (true) {
		ExMessage msg;
		while (peekmessage(&msg)) {
			if (msg.message == WM_MOUSEMOVE) {
				x = msg.x;
				y = msg.y;
			}
		}
		cleardevice();
		solidcircle(x, y, 100);
		FlushBatchDraw();
	}
	EndBatchDraw();
	return 0;
}
```





