步骤

其实只需要你去修改/定义 ~/.config/fontconfig/fonts.conf 这个文档就好。就我而言，我的配置：

    <?xml version="1.0"?>
    <!DOCTYPE fontconfig SYSTEM "fonts.dtd">
    <fontconfig>
    <!-- created by WenQuanYi FcDesigner v0.5 -->
    <match>
        <test name="family"><string>sans-serif</string></test>
        <edit name="family" mode="prepend" binding="strong">
            <string>WenQuanYi Micro Hei</string>
            <string>DejaVu Sans</string>
            <string>Microsoft Yahei</string>
        </edit>
    </match>
    <match>
        <test name="family"><string>serif</string></test>
        <edit name="family" mode="prepend" binding="strong">
            <string>DejaVu Serif</string>
            <string>WenQuanYi Bitmap Song</string>
        </edit>
    </match>
    <match>
        <test name="family"><string>monospace</string></test>
        <edit name="family" mode="prepend" binding="strong">
            <string>WenQuanYi Micro Hei Mono</string>
            <string>DejaVu Sans Mono</string>
        </edit>
    </match>
    </fontconfig>

当然前提是这些字体你都安装OK的。

修改完后，刷新下字体缓存： fc-cache -vf

取看看现在的默认字体吧 ： fc-match
