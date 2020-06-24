# 서브넷(Subnet)에 대하여

## IPv4

그냥 간단히 IP 주소의 버전 중 4번째라는 의미.

주소체계는 32-bit 길이이다.

세계에서 **유일한 값**이다.

주소는 **0.0.0.0부터 255.255.255.255까지**다.

이렇게 **.**(**dot**) 으로 표현하는 표기법을 Dotted-decimal notation이라고 부른다.

IPv4가 가질 수 있는 주소의 양은 2^32개이다.

<br/>

## Classful addressing

### Network Id

인터넷 상에서 모든 host들을 전부 관리하기 힘들기에 한 Network의 범위를 지정하여 관리하기 쉽게 만들어 낸 것.

### Host Id

호스트들을 개별적으로 관리하기 위해 사용하게 된 것

### Classful Address

> 사실 현재는 Classless Address를 사용하고 지금은 사용하지 않는 방식이다.

기존의 40억개의 ip주소 전체를 관리하기에 어려움이 있어서 주소를 class 단위로 나눠서 관리하는 방법이다.

![image](https://user-images.githubusercontent.com/48052622/85593052-a8f6bb80-b681-11ea-9da5-e71d42b0095c.png)

2^32개의 ip주소를 위의 그림과 같이 나눠서 사용하고 A, B, C, D, E 클래스로 나눠서 관리한다.

ip주소는 network id와 host id로 구성되어 있으며, ip주소에서 사용되는 Byte 공간은 아래와 같다.

![image](https://user-images.githubusercontent.com/48052622/85592971-954b5500-b681-11ea-8475-ad27eb101ab0.png)

![image](https://user-images.githubusercontent.com/48052622/85599333-60da9780-b687-11ea-9f73-dc863e223742.png)

#### Class A

ip주소는 0...의 형식으로 구성되어 있다.

network id는 1byte(8bit)를 사용하고 남은 3byte(24bit)를 host id로 사용한다.

(network id와 host id를 통해서 2^7개의 network 영역을 가지고 각 network마다 2^24개의 host ip를 사용함을 알 수 있다)

전체 ip주소에서 50%를 차지한다.

#### Class B

ip주소는 10...의 형식으로 구성되어 있다.

network id는 2byte(16bit)를 사용하고 남은 2byte(16bit)를 host id로 사용한다.

(network id와 host id를 통해서 2^14개의 network 영역을 가지고 각 network마다 2^16개의 host ip를 사용함을 알 수 있다)

전체 ip주소에서 25%를 차지한다.

#### Class C

ip주소는 110...의 형식으로 구성되어 있다.

network id는 3byte(24bit)를 사용하고 남은 1byte(8bit)를 host id로 사용한다.

(network id와 host id를 통해서 2^21개의 network 영역을 가지고 각 network마다 2^8개의 host ip를 사용함을 알 수 있다)

전체 ip주소에서 12.5%를 차지한다.

#### Class D, E

이 2종류의 클래스는 우리가 사용하는 영역은 아니다.

D 클래스는 multicasting을 위해, E 클래스는 연구 목적으로 사용된다.

### Network mask

앞서 각각의 클래스에서 network id로 사용되는 byte 영역을 network mask라고 한다.

예를 들어 Class B의 ip중 154.12.0.1라는 ip가 존재한다. 라우터에서 해당 ip로 통신하기 위해서는 ip가 속하는 network id가 필요하다.

Class B는 2byte가 netid이므로 network id는

10011010 00001100 00000000 00000001 // ip address (154.12.0.1)

11111111 11111111 00000000 00000000 // Class B의 Netid (255.255.0.0)

위 두 주소를 AND연산을 하면

10011010 00001100 00000000 00000000 이 결과로 나오게 되고 이는 154.12.0.0이다.

이렇게 network 주소를 걸러내는 방식이 masking이고 이때 쓰인 mask를 network mask라고 한다.

(여기선 255.255.0.0이 network mask이다)

### Subnet

이렇게 클래스를 단위로 ip를 받을 경우 너무 큰 영역을 받게 된다. 이를 해당 네트워크 내부에서 작은 단위로 잘라서 사용하는 방법이 **Subnetting**이다.

만약 183.240.0.0 이라는 network id가 있다고 하자. 해당 network id는 Class B에 해당한다. host id로 사용할 수 있는 개수는 2^16개인데 전체를 관리하기에는 너무 규모가 커서 2^14개 단위로 나눠서 사용하고 싶다면 subnet을 사용한다.

183.240.0.0 의 n etwork 내부 라우터가 **183.240.0.0**, **183.240.64.0**, **183.240.128.0**, **183.240.192.0** 에 해당하는 4개의 network id로 분리하고 각각의 네트워크에서 x.x.0.0~x.x.63.255, x.x.64.0~x.x.127.255 ... 와 같이 작은 단위의 네트워크로 사용할 수 있는 개념이다.

네트워크를 작게 나눠서 사용할 수 있다면 작은 네트워크들을 붙여서 사용하는 방법은 없을까?

### Supernet

기존에 Subnet이 큰 네트워크를 작은 단위로 잘라서 사용하는 방식이라면 **Supernetting**은 작은 네트워크들을 붙여서 사용하는 방식이다.

만약 어떤 회사가 Class C에 해당하는 ip를 할당받았다고 하자. 197.0.0.0의 network id를 받았다면 사용 가능한 ip는 197.0.0.0~255까지, 256개의 host id를 사용할 수 있다.

그러나 회사에서 필요한 host id의 개수가 1000개일 경우 Class B를 사용해야 하는데 그렇다면 낭비되는 host id의 개수가 너무 많다. 이때 Class C의 network id를 여러개 할당받는 것을 Supernetting이라고 한다.

회사는 197.0.0.0, 197.0.1.0, 197.0.2.0, 197.0.3.0에 해당하는 network id를 할당받는다면 사용 가능한 ip는

197.0.0.0~197.0.3.255까지, 1024개의 host id를 사용할 수 있다.

이런 경우 기존의 network를 4개 붙여서 사용했으므로 network mask도 2bit 증가시키도록 한다.

즉, Class C의 network mask는 11111111 11111111 11111111 00000000 (255.255.255.0) 인데 2^2개의 network id를 붙였으므로

11111111 11111111 **11111100** 00000000 (255.255.250.0) 와 같이 network mask를 사용하면 정상적으로 network id를 사용할 수 있다.

그러나 여기서도 연속한 network id만 할당이 가능하고 2^n개의 단위로 network id를 사야한다는 단점이 존재한다. 

<br/>

## Classless addressing

![image](https://user-images.githubusercontent.com/48052622/85606235-ee20ea80-b68d-11ea-9f9f-10ed2d692dd8.png)

ip 주소를 가변적인 크기로 할당받을 수 있는 방법이고 오늘날 주로 사용되는 방법이다.

서브넷 마스크(prefix의 길이)를 ip 주소에 붙여서 표기한다.

ip 주소가 243.123.48.12이고 network prefix가 26일 경우 243.123.48.12/26으로 표기한다.

이러한 표기 방법을 slash notation이라고 한다.

위의 예시의 경우 243.123.0.0이 network id가 된다.

<br/>

## 참고 링크

>[IPv4 Address & Classful addressing - alleysark](https://alleysark.tistory.com/64)
>
>[Supernetting & Classless addressing - alleysark](https://alleysark.tistory.com/68)