---
title: Undertale Yellow 汉化技术问题记录【5】
date: 2024-08-05 20:29:03
cover: /resources/images/item_use/cover.png
tags: 
 - Undertale
 - Undertale Yellow
 - 汉化
 - GameMaker
---

久违了各位，
最近因为实在太忙 所以没有出什么力
久违地打开待解决清单，首先就是这个
```非屠杀线使用回血物品旁白字叠在一起```的问题
![就像这样](./resources/images/item_use/cover.png)

# 寻踪
搜索文本找到函数```scr_item_use_text_yellow```
其中定义了变量```con_keep_previous```
当这个变量为```true```时在恢复HP文本前面添加换行
以及变量```con_message_number```记录对话数量
![code](./resources/images/item_use/code.png)

全局搜索```scr_item_use_text_yellow```
找到物体```obj_dialogue_battle_action_selected_item```
Step事件 在文本打完时检测是否符合要求 并自动触发计时器0
![code_use](./resources/images/item_use/code_use.png)

计时器0会自动进行回血 并进入下一句文本（也就是显示恢复HP）
![healing](./resources/images/item_use/healing.png)

Draw事件 当文本为最后一句 且```con_keep_previous```为true时
另外执行一个函数```scr_draw_text_effect_twitchy_textbox_battle_item_use```
![draw](./resources/images/item_use/draw.png)

这个函数会保留最后一句文本 并直接把恢复HP的文本叠在一起
是的没错 无脑叠在一起 而不是放在文本最后

# 修复
其实很简单 把这段执行```scr_draw_text_effect_twitchy_textbox_battle_item_use```的删掉
然后再回到```scr_item_use_text_yellow```
把自动添加的换行删掉即可

看似很简单 但是因为这个有病逻辑 害我找一下午
神经