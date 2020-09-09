# Ch02. How Bitcoin Works

<p align="center"><img src="./screenshot/03_01.png" width="800"></p>

>  비트코인 시스템의 전반적인 구성 요소

<br/>

## 커피 한 잔 구매하기

<p align="center"><img src="./screenshot/03_02.png" width="400"></p>

Alice 는 Joe 로부터 0.1BTC 을 송금받았습니다.  Alice 는 이후 Bob 의 가게에서 커피 한 잔을 구매할 계획입니다.

이는 판매 시점 정보 관리(POS) 시스템에 비트코인 옵션을 추가하면서 가능해졌습니다. POS 시스템은 달러로 나와있는 총 주문 가격을 일반적인 시세에 따라 비트코인으로 전환한 후 두 가지 통화 단위에 대한 가격을 보여줍니다. 또한 이 거래에 대한 지불 요정 (Payment Request) 이 들어있는 QR 코드도 보여줍니다.

<p align="center"><img src="./screenshot/03_03.png" width="200"></p>

이 지불 요청 QR 코드는 아래 URL 을 encoding 한 것입니다.

```
bitcoin:1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA?
amount=0.015&
label=Bob%27s%20Cafe&
message=Purchase%20at%20Bob%27s%20Cafe

Components of the URL

A bitcoin address: "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
The payment amount: "0.015"
A label for the recipient address: "Bob's Cafe"
A description for the payment: "Purchase at Bob's Cafe"
```

송금할 비트코인의 목적지 주소만 담겨있는 QR코드와 달리 위는 지불 요청 QR 코드로 인코딩 된 URL 로서 목적지 주소, 지불 금액 등 지불과 관련된 기본 내용을 담고 있습니다. 지불 요청을 통해 대금을 지불하는 데 사용되는 정보를 미리 넣어둘 수 있고, 동시에 사용자가 읽을 수 있는 설명도 제공합니다.

여기에서 비트코인 목적지 주소는 bob 의 bitcoin address 로서, bob의 wallet의 private key 로부터 생성되었습니다.

<br/>

## Transaction

Transaction 은 비트코인을 많이 보유한 소유자가 비트코인 일부를 다른 사람에게 전송하는 것을 승인한다고 네트워크에 말하는 것과 같습니다. 비트코인의 새로운 소유주는 또 다른 소유주에게 비트코인이 전송되는 것을 승인하는 다른 거래를 만드는 과정을 반복함으로써 매수한 비트코인을 소비할 수 있습니다. 

<p align="center"><img src="./screenshot/03_04.png" width="600"></p>

각 거래는 하나 이상의 **입력값(Input)** 과 **출력값(Output)** 으로 구성됩니다. 입력값과 출력값의 합계가 반드시 동일할 필요는 없습니다. 그러나 출력값의 총합은 입력값의 총합보다 약간 작아야 하며, 이 차이 값은 거래속에 포함된 **거래 수수료(implied transaction fee)** 가 됩니다.

> 거래 수수료란 공개 장부에 거래를 포함시킨 채굴자가 수거하는 소액의 지불금입니다.

또한 송금되는 비트코인의 금액 각각에 대한 소유권을 소유주의 디지털 서명을 통해 증명하는 과정이 거래에 포함되며, 누구든지 독립적으로 거래를 검증할 수 있습니다. **소비(Spending)** 란 이전 거래에서 송금되었던 돈이 비트코인 주소에 의해 확인된 새로운 소유주로 전송되는 거래에 서명을 함으로써 이루어지는 작업을 말합니다.



<p align="center"><img src="./screenshot/03_05.png" width="600"></p>

> 한 거래의 출력값이 새로운 거래의 입력값이 되는 chain 구조

거래를 통해 거래 입력값에서 거래 출력값으로 가치가 이동합니다. 입력값은 비트코인의 가치가 발생하는 지점으로, 주로 이전 거래의 출력값입니다. 거래 출력값은 키를 이용해서 새로운 소유주에게 비트코인의 가치를 넘겨줍니다.

즉 주소의 locking script, unlocking script 를 매칭시키는 것이 거래입니다.

### 거래 목록

1. Alice buying bitcoin from Joe with cash
2. Alice's payment to Bob's cafe uses UTXO from the previous transaction
3. Bob's payment to Gopesh uses UTXO from the precious transaction

<br/>

## Common Transaction

### Common Transaction Forms

<p align="center"><img src="./screenshot/03_06.png" width="500"></p>

가장 흔하게 볼 수  있는 거래 유형입니다. 하나의 주소에서 다른 주소로 단일 거래가 이루어지는 형태입니다. 이 경우 종종 원 소유주에게 돌려줘야 하는 '잔액' 이 존재하며, 이 때 하나의 입력값과 두개의 출력값이 발생합니다. (위의 거래목록 1에 해당)

<br/>

### Transaction aggregating funds

<p align="center"><img src="./screenshot/03_07.png" width="500"></p>

또 다른 일반 유형은 여러 개의 입력값을 하나의 출력값으로 합치는 거래입니다. 이 형태는 동전과 단위가 작은 지폐가 많이 있는 경우에 큰 단위의 지폐 한 장으로 교환하는 행위와 동일합니다. 지불 과정에서 잔액으로 받은 작은 단위의 금액을 정리하기 위해 이 유형의 거래가 시행됩니다.

### Distrubuting funds

<p align="center"><img src="./screenshot/03_08.png" width="500"></p>

하나의 입력값을 여러 명의 수신인에게 줄 수 있는 여러 개의 출력값으로 배분하는 형태입니다. 이 거래 유형은 기업체에서 다수의 직원들에게 급여를 지불하는 등 돈을 분배해야 할 경우에 사용됩니다.

<br/>

## Constructing a Transaction

### Getting the right inputs

앨리스의 지갑에서 밥에게 전송하기를 원하는 금액을 지불할 수 있도록 알맞은 입력값을 찾습니다. 지갑 대부분은 지갑 소유주가 보유한 키로 잠겨있는 **Unspent Transaction Output(UTXO)** 에 대한 DB 를 보유하고 있습니다. 따라서 앨리스의 지갑에는 현금과 비트코인을 교환했던 조 와의 거래에서 생성된 거래 출력값의 복사본이 포함되어 있습니다.



```
$ curl https://blockchain.info/unspent?active=1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK
```

> 앨리스의 비트코인 주소의 UTXO 를 전부 조사

위는 RESTful API 요청으로, 특정 비트코인 주소가 보유하고 있는 소비되지 않은(unspent) 거래 출력값 전부를 리턴하기 때문에 소비를 위해 필요한 거래 입력값을 생성하도록 필요한 정보를 제공합니다.

```
{

	"unspent_outputs":[

		{
			"tx_hash":"186f9f998a5...2836dd734d2804fe65fa35779",
			"tx_index":104810202,
			"tx_output_n": 0,
			"script":"76a9147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a888ac",
			"value": 10000000,
			"value_hex": "00989680",
			"confirmations":0
		}

	]
}
```

> 검색 결과

위 검색 결과를 통해 앨리스는 UTXO 가 한 개 있다는 사실을 알 수 있습니다. 이 검색 결과에는 해당 결과에 대한 참조가 포함되며, UTXO 가 들어있고, 금액 (1000만 사토시 = 0.1비트코인) 이 들어있습니다.

<br/>

### Constructing a Transaction

이제 거래를 전송해야 합니다. 