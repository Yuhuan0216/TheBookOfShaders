# [The Book Of Shaders](https://thebookofshaders.com/)
## Index
- [Begin](#begin)
#

## Begin

### - `Fragment Shader` 片段著色器 是什麼?
- 對每個像素顯示的計算指令, 代碼需要根據不同的位置執行不同的操作.
- 可以想像成一個函數, 輸入是位置, 輸出是顏色.

### - Shaders 如何做到快速的運行?
- `Parallel processing` 平行處理: 利用多個微處理器來處理各個像素的計算.
- `GPU 加速`: 硬體提供特定的數學函式計算, 不用透過軟體處理.

### - `GLSL` 是什麼?
- `GLSL`: openGL Shader Language, openGL 著色語言.
- 本篇參照 [Khronos Group](https://www.khronos.org/opengl/) 的規範執行
- 更多關於 [OpenGL](https://openglbook.com/chapter-0-preface-what-is-opengl.html) 歷史

### - Shaders 的學習困難之處
- 線程並行的`盲視`: 各線程間是獨立運行的. 數據在計算過程中以相同方向流動, 不能夠對其他線程的輸出進行檢查, 輸入進行修改.
- 線程的`無記憶性`: 線程在計算中無法記錄數據, 無法確定前一刻的狀態.
- 代碼的撰寫: 要求有通用規則, 達到依像素的不同位置輸出不同的結果.

### - `Hello World`
- 起手來個`色塊`渲染吧!

```glsl
#ifdef GL_ES
precision mediump float;
#endif

void main(){
    gl_FragColor = vec4(1.0, 0.0, 1.0, 1.0);
}
```

- 知識點:
    1. `GLSL`的語法編寫結構和`C`很像, 擁有其特徵.
    2. 像素的顏色是由 預設全域變數 `gl_FragColor` 所設定的.
    3. 語言有`變數(variables)`, `函數(functions)`, `數據(types)`.
    4. 範例的`vec4`是一個4維的顏色值, 分別對應到`紅, 綠, 藍, 透明`4種通道.
    5. 有`預處理巨集`(preprocessor macros), 巨集指令屬於預編譯的一部分(以`#`為開頭).
        - Ex: #ifdef, #endif.
    6. `float`在shader中相當泛用, 所以精度很重要. 低精度運算快, 高精度品質優. 
        - 範例中定義了所有`float`精度為`中(precision mediump float)`, 也可定為`低(precision lowp float)`, `高(precision highp float)`.
    7. `GLSL`不保證數值會被自動轉換型態, 因此在宣告變數時, 請給予對應的正確型態.
        - 範例中`vec4`準確到單精度浮點, 因此要賦值`float`, 請記得加上小數點.
    
### - `Uniforms`
- 因為顯示卡架構, 並行線程的輸入參數為`統一值(uniform)`且屬於`唯讀變數`.
- 在設定完 `shader` 精度後, 開始 `unifom 變數` 的定義.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

// 設定 unifom變數
uniform vec2 u_resolution;  // 解析度
uniform vec2 u_mouse;       // 像素點位置
uniform float u_time;       // 執行秒數
```

- 試試將`紅色通道`隨 `執行時間 uniform` 呈現 `sin波函數` 變化！

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;

void main() {
	gl_FragColor = vec4(abs(sin(u_time)), 0.0, 0.0, 1.0);
}
```

<!-- TODO:補上函數說明 -->
- `GLSL` 支援的`GPU加速(硬體加速)`函數運算:
    - [sin()](https://thebookofshaders.com/glossary/?search=sin) - 
    - [cos()](https://thebookofshaders.com/glossary/?search=cos) - 
    - [tan()](https://thebookofshaders.com/glossary/?search=tan) - 
    - [asin()](https://thebookofshaders.com/glossary/?search=asin) - 
    - [acos()](https://thebookofshaders.com/glossary/?search=acos) - 
    - [atan()](https://thebookofshaders.com/glossary/?search=atan) - 
    - [pow()](https://thebookofshaders.com/glossary/?search=pow) - 
    - [exp()](https://thebookofshaders.com/glossary/?search=exp) - 
    - [log()](https://thebookofshaders.com/glossary/?search=log) - 
    - [sqrt()](https://thebookofshaders.com/glossary/?search=sqrt) - 
    - [abs()](https://thebookofshaders.com/glossary/?search=abs) - 
    - [sign()](https://thebookofshaders.com/glossary/?search=sign) - 
    - [floor()](https://thebookofshaders.com/glossary/?search=floor) - 
    - [ceil()](https://thebookofshaders.com/glossary/?search=ceil) - 
    - [fract()](https://thebookofshaders.com/glossary/?search=fract) - 
    - [mod()](https://thebookofshaders.com/glossary/?search=mod) - 
    - [min()](https://thebookofshaders.com/glossary/?search=min) - 
    - [max()](https://thebookofshaders.com/glossary/?search=max) - 
    - [clamp()](https://thebookofshaders.com/glossary/?search=clamp) - 

### - `gl_FragCoord`

