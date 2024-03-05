# Práctica 1 - Seminario de Sistemas 1
### Sección N  2S2023
#### Brian Emmanuel Riad Matus Colocho - 201801290
#### Juan José López Pérez - 201908075
#### Alexis Marco Tulio López Cacoj - 201908359


---> Arquitectura


## Usuarios IAM
Para el uso de las instancias EC2, optamos por generar un usuario que tenga permisos de inicio/apagado/reinicio de las instancias __"Semi1 Practica1 Backend Python"__ y __"Semi1 Practica1 Backend Node.js"__. La conexión a esta en caso se deba modificar algo se hace a través de SSH directamente, con el par de llaves "semi1-main.pem". Para esto se creo un grupo de usuarios, se agrego al usuario "semi1prac1-ec2" a este, y al grupo se le adjunto la siguiente politica para permitir acceso a estas instancias y acceso a la consola.

```json
   {
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AllowInstancesStateTotalAccess",
			"Effect": "Allow",
			"Action": [
				"ec2:RebootInstances",
				"ec2:StartInstances",
				"ec2:MonitorInstances",
				"ec2:DescribeInstanceAttribute",
				"ec2:ModifyInstanceAttribute",
				"ec2:StopInstances",
				"ec2:DescribeSecurityGroupRules"
			],
			"Resource": [
				"arn:aws:ec2:us-east-1:351766445224:instance/i-0f5425eb3b6392b81",
				"arn:aws:ec2:us-east-1:351766445224:instance/i-025341c9522e6f206"
			]
		},
		{
			"Sid": "AllowInstancesListing",
			"Effect": "Allow",
			"Action": [
				"ec2:DescribeInstances"
			],
			"Resource": "*"
		},
		{
			"Sid": "AllowInstanceMonitoring",
			"Effect": "Allow",
			"Action": [
				"ec2:DescribeInstanceTypes",
				"ec2:DescribeInstanceEventNotificationAttributes",
				"ec2:DescribeInstanceStatus",
				"ec2:DescribeSecurityGroups"
			],
			"Resource": "*"
		},
		{
			"Sid": "AllowInstancesAlarmViewing",
			"Effect": "Allow",
			"Action": [
				"cloudwatch:DescribeAlarms"
			],
			"Resource": "arn:aws:cloudwatch:us-east-1:351766445224:alarm:*"
		}
	]
}
```

Por otro lado, para el bucket S3 que guarda las imagenes se creo otra politica, la cual permite todas las operaciones sobre el bucket en especifico, la politica es la siguiente


```json
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "VisualEditor0",
			"Effect": "Allow",
			"Action": [
				"s3:PutObject",
				"s3:GetObject",
				"s3:ListBucket",
				"s3:DeleteObject",
				"s3:GetBucketLocation",
				"s3:ListAllMyBuckets"
			],
			"Resource": "arn:aws:s3:::practica1-g4-paginaweb"
		}
	]
}
```

Junto a esta politica, se debe poner la politica general del bucket, para usuarios no autentificados y para multi-cuentas, etc. La politica del bucket es la siguiente:

```json
{
    "Version": "2012-10-17",
    "Id": "Policy1707712591110",
    "Statement": [
        {
            "Sid": "AllowPublicGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::practica1-g4-paginaweb/*"
        },
        {
            "Sid": "AllowPrivatePutObject",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::351766445224:user/semi1prac1-s3"
            },
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::practica1-g4-paginaweb/*"
        }
    ]
}
```


## Arquitectura

![image](https://github.com/Alexz330/torres_hannoi/assets/72354711/16a0768e-fdaa-4289-af90-2f269b85bed0)



## Capturas de pantalla

--->S3

![image](https://github.com/Alexz330/torres_hannoi/assets/72354711/0f3d5fbd-05b0-4c31-bb4f-00f49a5067bc)


![image](https://github.com/Alexz330/torres_hannoi/assets/72354711/e417092e-f7cd-4059-a4a3-a569868795e8)

---> RDS

![image](https://github.com/Alexz330/torres_hannoi/assets/72354711/889bf5ba-615f-47f0-a4a5-860300622e64)

---> Web

![image](https://github.com/Alexz330/torres_hannoi/assets/72354711/b0b2873c-d604-4666-bab9-1ce68dda9b4c)

