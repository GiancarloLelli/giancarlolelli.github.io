---
layout: post
title:  ".NET 7 è stato rilasciato"
date:   2022-11-28 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}

.NET 7 è stato finalmente rilasciato l'8 Novembre 2022. Una importante milestone per tutto il team.
![][image_ref_a6whysbe]
Questi in basso sono gli item di maggiore importante in questa major release del framework. Il dettaglio complete è disponibile nel blog ufficiale [qui](https://devblogs.microsoft.com/dotnet/announcing-dotnet-7/)

* **Unified**
    * One BCL
    * New TFMs
    * Native support for ARM64
    * Enhanced .NET support on Linux
* **Modern**
    * Continued performance improvements
    * Developer productivity enhancements, like container-first workflows
    * Build cross-platform mobile and desktop apps from same codebase
* **.NET is for cloud-native apps**
    * Easy to build and deploy distributed cloud native apps
* **Simple**
    * Simplify and write less code with C# 11
    * HTTP/3 and minimal APIs improvements for cloud native apps
* **Performance**
    * Numerous perf improvements

[image_ref_a6whysbe]: data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAYABgAAD/2wBDAAgGBgcGBQgHBwcJCQgKDBQNDAsLDBkSEw8UHRofHh0aHBwgJC4nICIsIxwcKDcpLDAxNDQ0Hyc5PTgyPC4zNDL/2wBDAQkJCQwLDBgNDRgyIRwhMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjIyMjL/wAARCAEuAu4DASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9oADAMBAAIRAxEAPwD3+iiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAoorlPiFreseHPDB1XR4Y5ngnRrhXXcPK53frjnsM1rRpSrVFTju9AOrorjm+J3ha30izvrzUkhkuoFmFsoMki5GcEKDj8cVnWnxj8OX99HZ2lpqs8sjbV8u2DZ/ANn9K6I5di5JtU3ZeQ7M9CopFO5Q2CMjOD1FLXEIKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACg5xx1oooArGC4cfPeMh/6YxqB/wCPbqpapaLHpszhppMD598jMCO/GcfpV+e7htx+8cZ/ujrVP+2bRgySq4UjByMgiq52ByukeDfCOoyStNoVo04O7O0jI+gOK6/TtG0vSEKabp1raKevkQqmfrgc1ylrdLpusbo33QhsZ9VNdg2oWijJnQ/Q5rSdetNcspNr1YXLNVb4X+xTYPbBwfmSdWw30IPy/kagk1q1Tpub8MVTl8RKARHGoPYs2azi7O4CTeI5NMCnWtOltELBFnhcTRsxOAAB8+f+A14/rHxF1jxl4ifSdN1iDw/payFBNNJ5TsAcZZuuf9kY969ES1kuLia4aO4vLiUFTLIm4op6qvGEH0A96ZL8ONC8QSTT6xpGydsETxuY3Y+p2nB/EV62CxODoTcqkLu2j3Sfo/8AP5DudL4Y0i30TQLezt7yS9XBdrqSTeZWPJbP8vatisTwz4W0/wAJ2Mllpr3Jgd9+yaUuFPt6Vt15leSlVlJO9+r0EFFFFZAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUjZ2nbjdjjPSquy/bO64gQHssJJH4lv6U0r9QMnxp4mbwj4ebVhZtdKkyI6Bs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