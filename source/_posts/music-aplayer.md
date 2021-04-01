---
title: Aplayer Custom element
date: 2021-04-01 14:12:45
tags: ["Music"]
aplayer: true
---

After setting front matter `aplayer: true`

You got the abilitiy to write [Aplayer Custom element](https://github.com/fengkx/svelte-aplayer) in markdown without `hexo-tag-aplayer`

And it is proatble in any environment in which you can write your own html code.

```html
<s-aplayer
  audio='[
  {"name": "光るなら",
    "artist": "Goose house",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/hikarunara.mp3",
    "lrc": "https://blog-static.fengkx.top/svelte-aplayer/hikarunara.lrc",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/hikarunara.jpg"
  },
  {"name": "神奇的糊塗魔藥",
    "artist": "林家謙",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/margic-sillines.mp3",
    "lrc": "https://blog-static.fengkx.top/svelte-aplayer/margic-silliness.lrc",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/margic-sillines.jpg"
  },
  {"name": "君の知らない物語",
    "artist": "supercell",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/bakemonogatari-ed.mp3",
    "lrc": "https://blog-static.fengkx.top/svelte-aplayer/bakemonogatari-ed.lrc",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/bakemonogatari-ed.jpg"
  },
  {"name": "「戦場ヶ原、蕩れ」",
    "artist": "神前暁",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/senjougahara.mp3",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/senjougahara.jpg"
  }]'
/>
```

HTML above produce

<s-aplayer
          audio='[
  {"name": "光るなら",
    "artist": "Goose house",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/hikarunara.mp3",
    "lrc": "https://blog-static.fengkx.top/svelte-aplayer/hikarunara.lrc",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/hikarunara.jpg"
  },
  {"name": "神奇的糊塗魔藥",
    "artist": "林家謙",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/margic-sillines.mp3",
    "lrc": "https://blog-static.fengkx.top/svelte-aplayer/margic-silliness.lrc",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/margic-sillines.jpg"
  },
  {"name": "君の知らない物語",
    "artist": "supercell",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/bakemonogatari-ed.mp3",
    "lrc": "https://blog-static.fengkx.top/svelte-aplayer/bakemonogatari-ed.lrc",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/bakemonogatari-ed.jpg"
  },
  {"name": "「戦場ヶ原、蕩れ」",
    "artist": "神前暁",
    "url": "https://blog-static.fengkx.top/svelte-aplayer/senjougahara.mp3",
    "cover": "https://blog-static.fengkx.top/svelte-aplayer/senjougahara.jpg"
  }]' />

You need to set cdn url in theme config file (`cdn.aplayer`) first checkout the [demo repo](https://github.com/fengkx/purer-theme-demo/blob/master/_config.theme.yml#L145)
