---
layout: post
title:  "Serverless App ง่ายๆบน AWS Lambda"
subtitle: "มารู้จักการ Serverless App / AWS Lambda + Mini Workshop"
date:   2017-01-16 02:15:41 +0700
categories: aws nodejs
image: /assets/images/thumbnail/js101.png
---

ในยุคปัจจุบันนั้นการ การดีพลอยเว็บก็ใช้วิธีการรันไว้บน server ทั่วไป ซึ่งจำเป็นต้องรันไว้ตลอดเวลาเพื่อให้สามารถเข้าถึงได้ตลอดเวลานั่นเอง เมื่อต้องรันไว้ตลอดเวลา ก็ต้องมี server รันค้างไว้ ถึงแม้จะไม่มีคนเข้าใช้งานก็ตาม แต่ก็ยังใช้ทรัพยากรนั่นไปเรื่อยๆอยู่ดี

แต่วันนี้เรามีแนวทางการทำเว็บใหม่มานำเสนอ นั่นก็คือ

## Serverless Application

ชื่อก็บอกอยู่แล้วนะ Serverless ก็คือ ไม่จำเป็นต้องมี server รันอยู่ตลอดเวลา โดยแอพที่เรารันนั้นจะรันเฉพาะเมื่อมี event ในการเรียกใช้งานเข้ามาเท่านั้น เช่น มีการเรียก HTTP Request เข้ามา หรือ มีค่าอัพเดตทาง DB แล้ว trigger เข้ามารันใช้งานแอพ ซึ่งแน่นอนว่าวิธีการเขียน การออกแบบ และการดีพลอยแอพ ก็จะแตกต่างออกไปจากเดิมค่อนข้างมากเลยทีเดียว ซึ่งทาง AWS ก็มี tools ตัวนึงที่จะมาใช้ให้เราสามารถสร้าง serverless application ได้ง่ายๆ บน aws เอง โดยมีชื่อว่า

## AWS Lambda

AWS Lambda เป็น Service ที่ไว้ใช้สำหรับสร้างและรัน Serverless Application ครับ ซึ่งจริงๆแล้วตัว Lambda เองเป็นแค่เครื่องมือกลางตัวนึง เพราะในการรัน serverless จริงๆนั่น aws จะใช้เครื่องมืออื่นๆอีกหลายๆอย่างมาประกอบกัน โดยมี Lambda เป็นตัวกลางในการจัดการนั่นเอง มาดูกันก่อนว่าจริงๆแล้ว มันประกอบไปด้วยอะไรบ้าง

### Architecture

AWS Lambda นั้นจะทำการรันโค้ดเราด้วย Container เมื่อมีการสั่งรันโปรแกรมที่เราจัดไว้ ตัว lambda จะสร้าง Container ขึ้นมาและทำการรันเฉพาะในการเกิดอีเวนท์ครั้งนั้น เมื่อโปรแกรม execute เสร็จแล้วก็จะเข้าสู่สถานะ Freeze เพื่อรอรับอีเวนท์ครั้งต่อไป

### Lambda Function

การเขียนโปรแกรมบน AWS Lambda นั้นจะเป็น Functional Programing โดยเขียนเฉพาะฟังก์ชันเที่เราต้องการจะรันในการเกิด Event แต่ละครั้ง โดยเราสามารถเขียนบนภาษาที่ Lambda รองรับได้ คือ `Node.js` `C#` `Java` `Python`

### Event Driven

การรัน Lambda Function ที่เราเขียนไว้นั้น จะเกิดขึ้นก็ต่อเมื่อมี Event มาทำการสั่งรันตัวฟังก์ชัน ซึ่งมีด้วยกันหลาย trigger โดยแบ่งเป็น 2 แบบหลักๆคือ

- Streaming Event

เป็นอีเวนท์ที่เข้ามาตลอดเวลาในช่วงระยะเวลาหนึ่งๆ เช่น ดึงข้อมูลจาก social network เป็นต้น โดยอีเวนท์นี้สามารถที่จะเชื่อมต่อกับเซอร์วิสของ AWS 2 ตัว คือ

	- DynamoDB Streaming
	- AWS Kinesis

- Non-Streaming Event

เป็นอีเวนท์ปกติที่สามารถ trigger ขึ้นมาด้วย action บางอย่าง เช่น การเรียก HTTP Request / IoT Event เป็นต้น โดยสามารถเชื่อมต่อกับ AWS Services ได้ดังนี้

	- API Gateway 	หรือสร้าง API ให้สามารถเรียกได้ผ่าน HTTP Request
	- AWS IoT
	- Cloudfront
	- Cloudwatch
	- S3
	- SNS

## Hello World API with Node.js

สำหรับท่านที่ยังไม่มีสมัคร account aws กันก่อนนะครับ จากนั้นไปที่ Console > AWS Lambda

จากนั้นก็มาเริ่มทำการสร้าง Lambda Function กันก่อนนะครับ


ในส่วนของการสร้างจะมี Blueprint ให้เราเลือกค่อนข้างเยอะ แต่ในบทความนี้เราจะมาดูตัวที่ง่ายที่สุดก่อนละกัน เลือกไปที่ `hello-world` เลยครับ


จากนั้นเราก็จะมาตั้งค่าตัว trigger สำหรับ invoke lambda function ตัวนี้นะครับ โดยในตอนนี้จะมาสร้าง API สำหรับให้เรียกใช้ได้ผ่าน Http Request นะครับ ซึ่ง AWS ก็จะมีเครื่องมือที่เรียกว่า `API Gateway` 

เมื่อเราเลือก API Gateway จากนั้นเราก็จะมาคอนฟิคตัว API กันนะครับ โดยมี 3 Option ให้เลือก

- API name
- Deployment Stage
- Security

โดย url ที่ได้จะอยู่ใน pattern 

จากนั้นเราก็จะมา config ตัว lambda function และเขียนโค้ด lambda function กันนะครับ ก็ตั้งชื่อและเลือก Runtime เลย โดยในที่นี้ผมจะใช้ Node.js ในการรัน ซึ่งเวอร์ชันที่ Lambda ใช้จะเป็น Node 4.3 

และในการเขียนโค้ดก็มีวิธีการใช้ดึงมาโค้ดมาใช้ได้หลายแบบ เช่น เขียนบนเว็บ อัพโหลดไฟล์ หรือดึงมาจาก S3 แน่นอนว่าตัวอย่างนี้แนะนำวิธีที่ง่ายที่สุดคือ เขียนบน AWS Console เลย

มาดูฟังก์ชันตั้งต้นกันก่อนนะครับ

```
'use strict';

console.log('Loading function');

exports.handler = (event, context, callback) => {
    //console.log('Received event:', JSON.stringify(event, null, 2));
    console.log('value1 =', event.key1);
    console.log('value2 =', event.key2);
    console.log('value3 =', event.key3);
    callback(null, event.key1);  // Echo back the first key value
    //callback('Something went wrong');
};
```

Lambda Function บน Node.js นั้นเราจะมีฟังก์ชันหลักที่จะเรียกใช้บน Lambda โดยและฟังก์ชนัจะมี parameter อยู่ 3 ตัว ส่ง response กลับไปด้วยฟังก์ชัน callback และหากเราจะส่ง response ผ่านทาง `API Gateway` เราก็ต้องมาดูกันก่อนว่า API Gateway นั้นรับพารามิเตอร์อะไรบ้าง เพื่อให้เราสามารถเขียน response ของ lambda function ให้ถูกฟอร์แมตของตัว API Gateway กันนะครับ ไม่อย่างนั้นจะไม่สามารถส่ง response กลับไปที่บราวเซอร์ได้ถูกต้องนะครับ

ฟอร์แมต Response ไปที่ตัว API Gateway ต้องเป็นประมานนี้

```
{
  "statusCode": 200,
  "headers": {},
  "body": ""
}
```

จากนั้นเราก็เขียน callback ให้ส่ง response กลับในรูปแบบนี้นะครับ

```
'use strict';

exports.handler = (event, context, callback) => {
	var str = "Hello World";

	callback(null, {
		"statusCode": 200,
		"headers": {},
		"body": str
	});
};
```

## API Gateway

