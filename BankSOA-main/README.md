# soap-bank (Spring Boot + Spring-WS)

## Build & Run
```bash
mvn clean package
mvn spring-boot:run
```

## WSDL
Open:
http://localhost:8080/ws/bank.wsdl

## Postman tests
- Method: POST
- URL: http://localhost:8080/ws
- Header: Content-Type: text/xml; charset=utf-8
- Body: raw (XML)

### GetAccount
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:ban="http://example.com/bank">
  <soapenv:Header/>
  <soapenv:Body>
    <ban:GetAccountRequest>
      <ban:accountId>A100</ban:accountId>
    </ban:GetAccountRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

### Deposit
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:ban="http://example.com/bank">
  <soapenv:Header/>
  <soapenv:Body>
    <ban:DepositRequest>
      <ban:accountId>A100</ban:accountId>
      <ban:amount>20.00</ban:amount>
    </ban:DepositRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

### Fault (amount <= 0)
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:ban="http://example.com/bank">
  <soapenv:Header/>
  <soapenv:Body>
    <ban:DepositRequest>
      <ban:accountId>A100</ban:accountId>
      <ban:amount>-5</ban:amount>
    </ban:DepositRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

### Withdraw
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:ban="http://example.com/bank">
  <soapenv:Header/>
  <soapenv:Body>
    <ban:WithdrawRequest>
      <ban:accountId>A100</ban:accountId>
      <ban:amount>50.00</ban:amount>
    </ban:WithdrawRequest>
  </soapenv:Body>
</soapenv:Envelope>
```

### Fault (Insufficient funds)
```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                  xmlns:ban="http://example.com/bank">
  <soapenv:Header/>
  <soapenv:Body>
    <ban:WithdrawRequest>
      <ban:accountId>A100</ban:accountId>
      <ban:amount>1000.00</ban:amount>
    </ban:WithdrawRequest>
  </soapenv:Body>
</soapenv:Envelope>
```
