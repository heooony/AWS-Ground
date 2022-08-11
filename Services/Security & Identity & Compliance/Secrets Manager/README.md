![Header (4)](https://user-images.githubusercontent.com/68142821/183649954-85df161d-7da4-4186-ad0e-9c5afe302cae.png)

# AWS Secrets Manager

<img width="100" alt="image" src="https://user-images.githubusercontent.com/68142821/183926274-04384dee-87f6-4131-a304-4bb0da0cbd14.png">

## 1. Secrets Manager

"일정 기간 동안 지나면 자동으로 비밀번호가 업데이트 되는 서비스"

> 최근 업데이트 : 2022.08.10

> Tip :
> 
> Secrets Manager은 무료가 아닙니다! 보안 암호당 $0.40/월API 호출 10,000건당 $0.05이라는 가격을 갖고 있으며, 30일간의 무료 평가판이 주어집니다.

> Tip :
> 
> AWS Secrets Manager는 ASM이라고 불리지 않습니다! ASM은 AWS Systems Manager의 약어입니다.

일정 기간이 지나면 비밀번호를 변경할 수 있습니다. 사용자가 직접 설정하는 것은 기간밖에 없습니다. 물론 다른 부분들도 설정이 필요로는 하지만, 수동적으로 직접 암호를 변경하지 않고, 설정한 기간에 따라 자동으로 암호를 변경시키는 부분에서 큰 장점을 얻습니다.

이 안에서 만든 키는 KMS을 사용해서 자동으로 암호화됩니다.

비밀번호를 변경하는 이유에 대해서는 알겠지만 이 서비스가 꼭 필요로 했던 이유는 무엇일까요?

![image](https://user-images.githubusercontent.com/68142821/183929968-dd3522e3-b372-46d1-bebb-63161758638c.png)

사진을 보면 어려울 수도 있지만 막상 뜯어보면 어렵지 않습니다!

1. Amazon에서 RDS서비스를 운영하고 있는데, 우리는 이러한 데이터베이스에 접근하기 위해 자격 증명 권한을 부여해야 합니다. 또한 데이터를 `MyCustomApp`에서 사용할 수 있도록 데이터베이스에서 자격 증명 권한을 부여해주어야 합니다.
2. 이를 통해 관리자는 자격 증명에 대한 이름과 함께 Secrets Manager에 업로드합니다.
3. MyCustomApp에서는 데이터베이스에 접근하고 싶은 경우 Secrets Manager에 업로드한 자격 증명에 대해 보안 암호를 쿼리합니다.
4. Secrets Manager은 보안 암호를 검색하고 보호되는 보안 암호 텍스트의 암호를 해독해 보안(TLS를 통한 HTTPS) 채널을 통해 클라이언트 앱에 보안 암호를 반환합니다.
5. 클라이언트 애플리케이션은 데이터베이스에 접근할 수 있습니다.