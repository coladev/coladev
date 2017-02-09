---
layout: post
title:  "มาลองสร้าง Serverless App บน AWS Lambda กัน"
subtitle: "รู้จักการทำ Serverless App / AWS Lambda + Mini Workshop"
date:   2017-01-16 02:15:41 +0700
categories: aws nodejs
image: /assets/images/thumbnail/lambda.png
---

&nbsp;&nbsp;&nbsp;&nbsp;Serverless เป็นเทรนใหม่ในการทำ Backend Application ในยุคปัจจุบันที่สามารถสร้างและขยายปริมาณการรองรับการใช้งานได้อย่างง่ายดาย และบน AWS Lambda นั่น สามารถช่วยให้เราทำ Serverless Application ในการจัดการข้อมูล รวมไปถึง API สำหรับแอพลิเคชันหน้าบ้านได้โดยแทบไม่ต้องใช้ความรู้ในการ setup และ deploy แอพในรูปแบบของเซิฟเวอร์แบบเดิมเลยทีเดียว เพียงแค่เขียนโปรแกรมให้ได้ตามที่ต้องการก็เพียงพอแล้ว

---



&nbsp;&nbsp;&nbsp;&nbsp;ในยุคปัจจุบันนั้นการ การดีพลอยเว็บใช้วิธีการรันไว้บนเซิฟเวอร์ทั่วไป ซึ่งจำเป็นต้องรันไว้ตลอดเวลาเพื่อให้สามารถเข้าถึงได้ตลอดเวลานั่นเอง เมื่อต้องรันไว้ตลอดเวลา ก็ต้องมีเซิฟเวอร์รันค้างไว้ ถึงแม้จะไม่มีการเข้าใช้งานก็ตาม แต่ก็ยังใช้ทรัพยากรนั่นไปเรื่อยๆอยู่ดี

แต่วันนี้เรามีแนวทางการทำเว็บใหม่มานำเสนอ นั่นก็คือ

## Serverless Application

ชื่อก็บอกอยู่แล้วนะ Serverless ก็คือ ไม่จำเป็นต้องมีเซิฟเวอร์รันอยู่ตลอดเวลา โดยแอพที่เรารันนั้นจะรันเฉพาะเมื่อมี event ในการเรียกใช้งานเข้ามาเท่านั้น เช่น มีการเรียก HTTP Request เข้ามา หรือ มีค่าอัพเดตทาง DB แล้ว trigger เข้ามารันใช้งานแอพ ซึ่งแน่นอนว่าวิธีการเขียน การออกแบบ และการดีพลอยแอพ ก็จะแตกต่างออกไปจากเดิมค่อนข้างมากเลยทีเดียว ซึ่งทาง AWS ก็มีเครื่องมือ ตัวนึงที่จะมาใช้ให้เราสามารถสร้าง serverless application ได้ง่ายๆ บน aws เอง โดยมีชื่อว่า

## AWS Lambda

AWS Lambda เป็นเครื่องมือที่ไว้ใช้สำหรับสร้างและรัน Serverless Application บน AWS ครับ โดยเจ้า Lambda เองเป็นเครื่องกลางในการผนวก service อื่นๆของอเมซอนเข้ามาใช้ในการรันแอพลิเคชัน มาดูกันก่อนว่าจริงๆแล้ว ตัว AWS Lambda เอง มันประกอบไปด้วยอะไรบ้าง และสามารถเชื่อมต่อกับ service อื่นๆตัวไหนของ aws ได้อีกบ้าง

### Architecture

การรันโปรแกรมบน AWS Lambda นั้น ตัว Lambda จะแปลงโค้ดที่เราจะต้องการรัน หรือเรียกสั่งๆว่าเป็น `Lambda Function` เป็น Container เมื่อมีการรันโปรแกรมผ่านช่องทางที่เราผูก event ไว้ ตัว lambda เองก็จะรันตัวโปรแกรมนั้นให้เราจบๆไปเป็น event และเมื่อไม่มีการเรียกใช้งานก็จะอยู่ในสถานะ freeze ตัวเอง หรือไม่ได้รันเซิฟเวอร์อยู่นั่นเอง

![AWS Lambda Architecture for Web Application](/assets/images/posts/lambda/lambda-architect.png)

ยกตัวอย่างการทำเว็บแอพลิเคชันเชื่อมต่อกับ AWS Lambda ซึ่งทำตัวเป็น API ตัวนึง ก็จะเกิดการรันก็ต่อเมื่อมี Request เข้ามาที่ตัวระบบ

### Lambda Function

การเขียนโปรแกรมบน AWS Lambda นั้นจะเป็น Functional Programming โดยเขียนเฉพาะฟังก์ชันเที่เราต้องการจะรันในการเกิด Event แต่ละครั้ง โดยเราสามารถเขียนบนภาษาที่ Lambda รองรับได้ คือ `Node.js` `C#` `Java` `Python`

### Event Driven

การรัน Lambda Function ที่เราเขียนไว้นั้น จะเกิดขึ้นก็ต่อเมื่อมี Event มาทำการสั่งรันตัวฟังก์ชัน ซึ่งมีด้วยกันหลาย trigger โดยแบ่งเป็น 2 แบบหลักๆคือ

- Streaming Event

เป็นอีเวนท์ที่เข้ามาตลอดเวลาในช่วงระยะเวลาหนึ่งๆ เช่น ดึงข้อมูลจาก social network เป็นต้น โดยอีเวนท์นี้สามารถที่จะเชื่อมต่อกับเซอร์วิสของ AWS 2 ตัว คือ

	- DynamoDB Streaming
	- AWS Kinesis

- Non-Streaming Event

เป็นอีเวนท์ปกติที่สามารถ trigger ขึ้นมาด้วย action บางอย่าง เช่น การเรียก HTTP Request / IoT Event เป็นต้น โดยสามารถเชื่อมต่อกับ AWS Services ได้ดังนี้

	- API Gateway 	สำหรับสร้าง API ให้สามารถเรียกได้ผ่าน HTTP Request
	- AWS IoT
	- Cloudfront
	- Cloudwatch
	- S3
	- SNS

![AWS Lambda S3](/assets/images/posts/lambda/lambda-photo.png)

ยกตัวอย่างการใช้งานรวมกับ S3 เมื่อมีรูปถูกอัพโหลดขึ้นมาที่ S3 ก็จะมี Event ตัวนึงไป trigger เพื่อรัน lambda function ไปทำการ resize รูป

## Hello World API with Node.js

สำหรับท่านที่ยังไม่มีสมัคร account aws กันก่อนนะครับ จากนั้นไปที่ Console > AWS Lambda แล้วเราก็จะมาเริ่มทำการสร้าง Lambda Function กันก่อนนะครับ

![AWS Lambda S3](/assets/images/posts/lambda/lambda-blueprint.png)

ในส่วนของการสร้างจะมี Blueprint ให้เราเลือกค่อนข้างเยอะ แต่ในบทความนี้เราจะมาดูตัวที่ง่ายที่สุดก่อนละกัน เลือกไปที่ `hello-world` เลยครับ


![AWS Lambda Trigger](/assets/images/posts/lambda/lambda-trigger.png)

จากนั้นเราก็จะมาตั้งค่าตัว trigger สำหรับ invoke lambda function ตัวนี้นะครับ โดยในตอนนี้จะมาสร้าง API สำหรับให้เรียกใช้ได้ผ่าน Http Request นะครับ ซึ่ง AWS ก็จะมีเครื่องมือที่เรียกว่า `API Gateway` 

![AWS Lambda + API Gateway](/assets/images/posts/lambda/lambda-api-gateway.png)

เมื่อเราเลือก API Gateway จากนั้นเราก็จะมาคอนฟิคตัว API กันนะครับ โดยมี 3 Option ให้เลือก

	- API name 			// Group Service
	- Deployment Stage		// Stage
	- Security

โดย Security จะมีให้เลือกอยู่ 3 option ตั้งเชื่อ API name และ deployment stage ได้ตามสะดวก

	- AWS IAM
	- Open
	- Open with access key

ในบทความนี้เราจะทำ API ที่เป็น Public ก่อน ดังนั้นเลือก Open เลยครับ 


![AWS Lambda Function Detail](/assets/images/posts/lambda/lambda-function-detail.png)

จากนั้นเราก็จะมา config ตัว lambda function และเขียนโค้ด lambda function กันนะครับ ก็ตั้งชื่อและเลือก Runtime เลย โดยในที่นี้ผมจะใช้ Node.js ในการรัน ซึ่งเวอร์ชันที่ Lambda ใช้จะเป็น Node 4.3 และในการเขียนโค้ดก็มีวิธีการใช้ดึงมาโค้ดมาใช้ได้หลายแบบ เช่น เขียนบนเว็บ อัพโหลดไฟล์ หรือดึงมาจาก S3 แน่นอนว่าตัวอย่างนี้แนะนำวิธีที่ง่ายที่สุดคือ เขียนบน AWS Console เลย โดยผมเลือก option เป็น default ทั้งหมดเลย

การตั้งชื่อ API name / deployment stage และ function name จะมีผลกับ url ของ API Gateway ที่เราจะเรียกใช้ service นี้เช่นกัน โดยเราจะได้ url ออกมาในรูปแบบ

```
http://url/[API NAME]/[DEPLOYMENT STAGE]/[FUNCTION NAME]
```

จากนั้นก็กดสร้างก็จะได้ Lambda Function ไว้ช้งานเรียบร้อย

![AWS Lambda Function](/assets/images/posts/lambda/lambda-edit-response-console.png)


มาดูในส่วนของโค้ดฟังก์ชันกันก่อนนะครับ

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

Lambda Function บน Node.js นั้นเราจะมีฟังก์ชันหลักโดยจะมี parameter อยู่ 3 ตัว ส่ง response กลับไปด้วยฟังก์ชัน callback และหากเราจะส่ง response ผ่านทาง `API Gateway` เราก็ต้องมาดูกันก่อนว่า API Gateway นั้นรับพารามิเตอร์อะไรบ้าง เพื่อให้เราสามารถเขียน response ของ lambda function ให้ถูกฟอร์แมตของตัว API Gateway กันนะครับ ไม่อย่างนั้นจะไม่สามารถส่ง response กลับไปที่บราวเซอร์ได้ถูกต้องนะครับ

ฟอร์แมต Response ไปที่ตัว API Gateway ต้องเป็นประมานนี้

```
{
  "statusCode": 200,		// HTTP Status
  "headers": {},		// Header
  "body": ""		//String
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

ทีนี้มาลองรับค่า parameter โดยรับค่าผ่านตัวแปร `event` แล้วส่งค่าเป็น json response กลับไปผ่าน header `Content-Type` กันนะครับ

```
'use strict';

exports.handler = (event, context, callback) => {
	var a = parseInt(event.a);
	var b = parseInt(event.b);

	var sum = parseInt();
	var response = {
		equation: a + " + " + b,
		sum: a + b
	}

	callback(null, {
		"statusCode": 200,
		"headers": {
			"Content-Type": "application/json"
		},
		"body": JSON.stringify(response)
	});
};
```

## API Gateway

