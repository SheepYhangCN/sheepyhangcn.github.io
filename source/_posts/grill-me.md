---
title: 持续拷问用户的 Skill：grill-me
date: 2026-07-14 18:10:33
repo: mattpocock/skills
---
Vibe Coding 有一个非常难受的点
就是 LLM 经常会因为 prompt 不够明确等原因而误解意思
或是在没有强调的地方自作主张

而看着它 reasoning 思考内容发觉到这一点的用户
要么跟个无能的丈夫一样看着做完再让它改
要么就只能无奈强制结束并 restore
然后再无限修改自己的 prompt 补全强调

这太抽象了，就不能让它多问问吗
然后我就找到了这个 Skill：[grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)（[grilling](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md)）
一个会在开始前“拷问”用户的 Skill

## 安装 Skill
打开 [Github 仓库](https://github.com/mattpocock/skills) 的 `skills/productivity` 目录
把里面的 `grilling` 和 `grill-me` 拷出来就可以了
直接上传到对应 Vibe 工具的 Skill 目录
对我来说是放在 `%userprofile%\.agents\skills`

## How2use
提出要求时使用这个 Skill
```
/grill-me <你的需求 Prompt>
```
LLM 确定需求后就会总结出一些问题
例如需求中不太明确的点 某些步骤的设计决策等
用户给出回答并确认后才会开始修改

## 等待深度使用
目前简单用下来还是不错的
我以前 Vibe 的一大痛点就是 LLM 不清楚也不问
天天自由发挥
所以对我来说还是用处很大的
详细的也许得等我深度使用之后再说了