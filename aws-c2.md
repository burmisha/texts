Итак, сегодня воскресенье, 4 октября 2020 и я опять куда-то дел пароли от инстансов в AWS.

Логинимся в AWS-консоль и идём в https://us-west-2.console.aws.amazon.com/ec2
EC2 — это Elastic Compute Cloud.
Там создаём новый образ: 
* я взял свежую LTS Ubuntu, 
* самый маленький образ,
* согласился создать для неё новую пару ключей, 
* скачал себе приватный (и положил в базу паролей, ибо надоело).

Не забудьте выставить права в Security, а то потом ещё не сможете зайти.

Inbound rules
Type 	Protocol 	Port range 	Source 
HTTP	TCP			80			0.0.0.0/0
HTTP	TCP			80			::/0
HTTPS	TCP			443			0.0.0.0/0
HTTPS	TCP			443			::/0
SSH		TCP			22			0.0.0.0/0


```shell
mv ~/Downloads/YOUR_NEW_AMAZON_PRIVATE_KEY.pem ~/.ssh/
chmod 600 ~/.ssh/YOUR_NEW_AMAZON_PRIVATE_KEY.pem
ssh ubuntu@<Public IPv4 DNS>
# там можно будет сделать sudo su и стать root-ом
```

https://medium.com/appgambit/part-1-running-docker-on-aws-ec2-cbcf0ec7c3f8
https://medium.com/@darryleong95/how-to-host-your-telegram-bot-on-an-ec2-instance-windows-guide-6a4569eacd99
