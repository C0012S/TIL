# 설정
- IAM User 생성
1. Access type은 Programmatic access으로 설정
2. Set Permissions - Attach existing policies directly 선택 후 AmazonRekognitionFullAccess, AmazonS3FullAccess 선택
3. Access key ID, Secret access key 확인 및 저장 (Download .csv)

<br/>

- S3 Bucket 생성 후 Image 업로드

<br/>

- 다운로드 한 csv 파일을 코드 파일과 같은 폴더에 넣어 둔다.

<br/>
<br/>

# 이미지 조절(Image Moderation)

```python
import csv
import boto3

with open('credentials.csv', 'r') as input:
    next(input)
    reader = csv.reader(input)
    for line in reader:
        access_key_id = line[2]
        secret_access_key = line[3]

photo = 'photo_name.jpg'

client = boto3.client('rekognition',
                      aws_access_key_id = access_key_id,
                      aws_secret_access_key = secret_access_key,
                      region_name="ap-northeast-2") # S3 Bucket의 AWS region과 동일하게 설정

response = client.detect_moderation_labels(Image={'S3Object': {
            'Bucket': 'bucket_name',
            'Name': photo,
        }}
                                #, MinConfidence=95 # MinConfidence가 95보다 작으면 ModerationLabels 없다
                                                    )

print(response)
```

<br/>
<br/>

# 참고 자료
- Tutorial : [Image Recognition with AWS](https://www.youtube.com/playlist?list=PLqEbL1vopgvv1gkA2Gz-yQzKUyja4D2ew)
- Demo : [Amazon Rekognition Demo](https://ap-northeast-2.console.aws.amazon.com/rekognition/home?region=ap-northeast-2#/label-detection)
- 공식 문서 - 방법 : [이미지 작업](https://docs.aws.amazon.com/ko_kr/rekognition/latest/dg/images.html)
- 공식 문서 - 함수 : [Rekognition - Boto3 Docs 1.20.49 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rekognition.html#Rekognition.Client.detect_labels)

