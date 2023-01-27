# Konachan-CHS-Browse

# 使用方法

    当然别忘记npm install，需要有node相关东西

    本程序是作为 https://gelbooru.wjcodes.com/ 的浏览模式来使用的，类似于konachan.net和konachan.com的Browse模式，只不过前者没有色图，后者需要翻墙。

    使用中只需注意搜索用英文逗号分隔，单个词语中间的空格用_代替，例如hatsune miku应写为hatsune_miku，分级模式的应使用rating:safe（全年龄），rating:questionable（15X），rating:explicit（18X）

# 特别说明

    图片加载时好时坏是正常的，请刷新或者一直下一张图可能会有能够加载的部分，我并不清楚为什么会无法加载，看网络连接是被重定向到了konachan.com的本图片的页面（HTML），有能力的人如果可以帮忙分析一下并提出解决思路就更好了

    App.vue中findImageUrl和imageUrls有两套方式，被注释的更加省流量，但似乎失败率会更高（不确定），
    还有被注释的刷新功能，此功能会导致图片稳定无法加载，原因不明