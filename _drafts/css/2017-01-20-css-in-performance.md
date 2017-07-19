---
layout: post
title:  "เขียน CSS ยังไงให้มีประสิทธิภาพดีที่สุด"
subtitle: "เข้าใจหลักการทำงานของบราวเซอร์และวิธีการเขียน CSS ให้ได้ประสิทธิภาพ"
date:   2017-01-16 02:15:41 +0700
categories: javascripts framework
image: /assets/images/thumbnail/js101.png
---

ทุกวันนี้ Front-end Dev หลายๆคนอาจจะมองข้ามเรื่อง Performance ในการเขียน CSS เนื่องจากประสิทธิภาพของเครื่องทุกวันนี้ค่อนข้างสูง และไม่จำเป็นต้องไปกังวลกับเรื่องความหน่วงหรือกระตุกมากนักหากไม่ได้เขียนท่าอะไรประหลาดๆมาก แต่หากเราจำเป็นต้องเขียนให้ได้ประสิทธิภาพสูงสุดละ เราต้องเข้าใจอะไรบ้าง มาดูกัน

## How CSS Work in Browser

### Query Selector


#### Performance Result

### Repaint

Repaint เป็นการเปลี่ยนแปลง CSS ในระดับ Node นั้น จะเกิดขึ้นเมื่อเราเปลี่ยนแปลงค่า Properties ที่ไม่ได้มีผลกระทบอะไรกับ Layout ของเว็บ เช่น

### Reflow

Reflow เป็นการเปลี่ยนแปลง CSS ที่บราวเซอร์จำเป็นต้องคำนวนการเปลี่ยนแปลง Layout ของเว็บใหม่ ส่งผลให้บราวเซอร์ต้องทำการคำนวน render ของ Node อื่นๆไปด้วย ซึ่งจะส่งผลต่อประสิทธิภาพของเว็บได้ เช่น

`font-size`


#### Avoid Repaint & Reflow

แน่นอนว่าเมื่อเราทำการเปลี่ยนแปลงค่า CSS Properties เราไม่สามารถหลีกเลี่ยง Repaint ได้อยู่แล้ว แต่หลักๆที่เราจำเป็นต้องหลีกเลี่ยงเลยคือ Reflow เพราะจำทำให้เว็บช้าลง โดยเราสามารถหลีกเลี่ยงได้ด้วยวิธีการเหล่านี้

##### หลีกเลี่ยงการเปลี่ยนแปลง css properties เหล่านี้

- font-size
-

## CSS Debugging Tools in Chrome


## References

- http://javascript.tutorialhorizon.com/2015/06/06/what-are-reflows-and-repaints-and-how-to-avoid-them/