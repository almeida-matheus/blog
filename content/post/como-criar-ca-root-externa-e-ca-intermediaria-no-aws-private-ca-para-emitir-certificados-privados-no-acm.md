+++
title = "Como criar CA Root e CA Intermediária no AWS Private CA para emitir certificados privados na AWS"
date = 2024-05-05T00:47:11-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Gere uma autoridade certificadora para emitir certificados privados via AWS ACM"
categoria = [
    "Segurança",
]
+++

O objetivo deste artigo é ensinar como criar uma CA root externa e uma CA intermediária pelo AWS Private CA para que seja possível emitir certificados privados na AWS.

Para gerar os certificados será necessário o uso da ferramenta `openssl`, caso não tenha instalada use o seguinte comando para instalar:

```
sudo apt install openssl
# or
sudo yum install openssl
```

Crie um ambiente local para a criação dos componentes do certificado:

```
mkdir ca
touch ca/root-openssl.cnf
touch ca/intermediate-ext.cnf
touch ca/intermediate-request.csr
cd ca
```

## Criar certificado root externo

Primeiramente devemos criar o arquivo de configuração openssl personalizado para gerar o certificado com as devidas informações e as extensões.

As extensões irão definir a utilidade do certificado, para mais informações sobre essas extensões eu recomendo a leitura da [documentação oficial do openssl](https://medium.com/r?url=https%3A%2F%2Fwww.openssl.org%2Fdocs%2Fman3.0%2Fman5%2Fx509v3_config.html).

### 1. Preencha o arquivo `root-openssl.cnf` com o conteúdo:

```
[ req ]
distinguished_name              = req_distinguished_name
policy                          = policy_match
x509_extensions                 = v3_ca

[ policy_match ]
countryName                     = optional
stateOrProvinceName             = optional
organizationName                = optional
organizationalUnitName          = optional
commonName                      = supplied
emailAddress                    = optional

[ req_distinguished_name ]
countryName                     = Country Name (2 letter code)
countryName_default             = BR
countryName_min                 = 2
countryName_max                 = 2
stateOrProvinceName             = State or Province Name (full name)
stateOrProvinceName_default     = State
localityName                    = Locality Name (eg, city)
localityName_default            = City
0.organizationName              = Organization Name (eg, company)
0.organizationName_default      = Organization
organizationalUnitName          = Organizational Unit Name (eg, section)
organizationalUnitName_default  = Organizational Unit
commonName                      = Common Name (eg, your name or your server hostname)
commonName_max                  = 64
emailAddress                    = Email Address
emailAddress_max                = 64

[ v3_ca ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical,CA:true
keyUsage = critical,keyCertSign,digitalSignature,cRLSign
```

### 2. Gere a senha para criptografar a chave privada

```
openssl rand -base64 32 > root-password-encrypted.txt
```

O comando irá gerar 32 caracteres aleatórios para criar uma senha que será utilizada na criptografia da chave privada.

### 3. Gere a chave privada criptografada

```
openssl genrsa -aes256 -out root-encrypted.key -passout file:root-password-encrypted.txt 4096
```

O comando irá gerar uma chave privada RSA criptografada de 4096 bits usando o algoritmo de criptografia AES256.

### 4. Gere o certificado da CA root

```
openssl req -new -x509 -days 7300 -config root-openssl.cnf -key root-encrypted.key -passin file:root-password-encrypted.txt -out root-certificate.pem
```

O comando irá gerar o certificado root de autoassinado com base na chave privada descriptografada com tempo de expiração de 20 anos (7300 dias).

Ao executar o comando será solicitado o preenchimento das informações do certificado como CN, OU, etc.

Podemos usar o comando abaixo para visualizar o conteúdo do certificado em texto simples:

```
openssl x509 -in root-certificate.pem -text -noout
```

A saída desse comando será semelhante a:

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            70:5a:46:4a:85:a6:20:51:4b:21:47:dc:1e:4a:d5:98:a9:c6:4b:d2
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = BR, ST = Minas Gerais, L = Belo Horizonte, O = Me, OU = Me, CN = Root CA
        Validity
            Not Before: May  5 19:05:30 2024 GMT
            Not After : Apr 30 19:05:30 2044 GMT
        Subject: C = BR, ST = Minas Gerais, L = Belo Horizonte, O = Me, OU = Me, CN = Root CA
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (4096 bit)
                Modulus:
                    00:c4:04:99:9e:f9:aa:13:02:b5:8e:19:49:44:77:
                    8c:4d:89:c6:0b:86:6b:6e:51:d7:0b:38:e2:69:aa:
                    f2:2c:ff:21:fd:b1:8b:f2:65:d9:0c:69:a2:f6:b0:
                    53:7c:92:9a:6d:ed:8a:3d:ce:6a:9d:2f:92:c4:17:
                    df:83:ef:a1:37:4d:c0:22:bd:85:55:7a:bd:81:a7:
                    02:31:50:08:49:ec:40:00:46:3c:16:ed:54:d8:9e:
                    db:9b:03:d8:75:e9:0d:54:81:da:de:98:87:aa:6c:
                    87:b8:fe:98:5f:d8:8d:22:a1:86:3d:03:ad:32:3d:
                    61:8e:bb:32:75:78:2a:e8:a7:4a:27:93:2b:09:de:
                    0a:f5:e2:4f:ae:d1:88:0e:11:42:82:da:31:ab:4c:
                    45:db:a6:60:c8:54:a0:6b:79:79:77:73:ad:e4:79:
                    7e:66:58:eb:a9:eb:9a:28:89:bf:85:76:93:42:53:
                    8c:f9:8b:d3:a5:88:aa:db:d9:ab:b6:82:76:f9:fd:
                    76:06:17:41:16:de:83:94:60:5c:d7:96:cd:87:76:
                    20:22:52:e1:6b:dc:f6:d1:b5:76:b3:01:03:7c:a0:
                    36:d4:d1:5b:e8:14:1a:60:e2:26:71:89:fc:aa:c8:
                    d9:25:66:0e:72:7b:5a:5d:02:86:11:05:44:0f:d2:
                    09:8e:51:34:0d:85:58:e2:e6:9e:ad:4e:04:1f:c0:
                    8b:0a:44:9a:b0:73:a2:0c:fc:1c:61:51:04:70:47:
                    7f:d2:d3:bd:b1:ce:8f:26:63:9c:63:8a:73:6a:e4:
                    7b:29:19:b6:1f:e1:3e:18:a0:a3:c9:75:46:06:f0:
                    06:85:e0:4d:87:03:84:b1:11:ba:c0:50:f9:ac:dd:
                    29:ff:c2:94:c5:59:02:61:37:c1:f2:0c:79:5d:ef:
                    96:7d:a8:ab:e3:c8:94:05:a1:e0:19:4a:3b:ad:37:
                    db:02:51:39:fa:0d:96:c3:88:d7:98:9c:6d:5e:62:
                    11:4f:44:58:26:1b:3c:5c:56:58:b4:f2:65:08:f4:
                    02:b8:11:44:76:01:62:b7:76:e7:fd:de:f4:3f:c9:
                    02:1f:e1:95:71:03:12:26:d0:78:23:0c:53:35:1d:
                    fd:9b:45:37:8e:46:96:f5:66:d4:a4:b5:36:79:d6:
                    50:0c:f1:e3:89:b4:79:32:09:5e:11:72:5c:65:34:
                    6c:9c:a3:45:8a:a7:04:6f:c7:88:d8:ac:93:ec:e9:
                    57:62:a3:9a:de:43:d9:72:63:40:c1:7b:dc:ba:cf:
                    2f:35:db:70:68:ed:1c:c6:78:89:8b:f4:29:da:e4:
                    79:4a:27:38:71:d8:61:4a:f9:dd:5f:8f:33:45:f4:
                    40:20:bd
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Subject Key Identifier:
                61:51:2D:F8:F0:95:C5:FF:FE:96:A8:2C:E0:85:ED:58:E5:7F:0A:84
            X509v3 Authority Key Identifier:
                keyid:61:51:2D:F8:F0:95:C5:FF:FE:96:A8:2C:E0:85:ED:58:E5:7F:0A:84
X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Key Usage: critical
                Digital Signature, Certificate Sign, CRL Sign
    Signature Algorithm: sha256WithRSAEncryption
         41:91:9e:85:54:bd:44:49:86:94:4c:d4:fb:8b:9e:c3:32:4f:
         6d:a3:8f:ea:e4:0f:b0:a2:97:24:56:64:3a:e6:99:15:2f:31:
         96:6c:ed:6f:cf:0a:8d:f8:1c:38:db:41:86:64:22:52:19:fd:
         3b:ff:22:92:37:f8:05:6e:0a:7c:f2:4b:ba:4c:5f:bf:5b:18:
         8f:f5:38:0c:22:49:15:de:17:99:e1:c5:69:3a:0f:d9:18:4d:
         c0:c5:6e:ff:32:35:df:e1:7e:c1:e7:0f:7f:b6:ed:a6:e2:0d:
         2f:cd:29:65:14:94:3d:90:79:e5:a7:3e:93:a6:d4:92:2a:b6:
         62:9f:41:20:ed:23:80:28:3d:80:cd:d4:47:bd:06:d3:6e:35:
         c9:28:6e:21:87:c7:72:39:c4:1c:66:fa:21:c2:73:f6:dd:ba:
         16:99:84:11:fb:91:f0:40:17:38:5a:24:c1:d2:2d:8d:be:76:
         34:05:95:f0:f3:bf:b2:5b:26:05:d1:28:5f:b8:e2:cb:db:d7:
         79:29:fe:8e:c5:4e:ac:97:55:f0:fd:37:42:f3:f0:23:27:f9:
         24:b4:92:f0:d0:23:63:10:52:82:55:61:fe:7a:c4:5f:5b:a8:
         7c:88:16:e2:8e:f1:72:bf:56:8b:23:c4:93:5b:3b:4b:d5:e9:
         e1:f4:bb:26:d4:2c:32:7c:f7:6a:a9:f3:42:21:a3:f4:5b:d3:
         f1:59:74:67:13:59:e6:81:3c:88:a2:2a:3a:25:b3:df:b2:b9:
         fd:b3:95:36:f4:18:bf:a1:51:b6:1d:c8:03:cc:a2:e6:2b:99:
         bb:36:68:48:88:96:31:f0:db:7f:f0:48:57:da:bc:dd:f4:f6:
         53:1b:57:7a:5f:16:ab:1b:f5:ff:a0:96:30:5f:8d:57:b5:b0:
         7c:f2:a2:40:27:6d:e6:20:5f:13:79:3c:5b:ac:5d:78:40:59:
         24:e2:91:5f:ac:e1:f2:c0:b7:87:b7:d0:66:78:06:6e:1b:77:
         b3:64:2d:62:14:e1:ec:cf:c3:2a:cb:38:08:87:04:11:ca:03:
         e6:a8:e1:4e:c4:13:ad:9b:ed:15:80:58:44:81:50:17:cf:ae:
         84:32:0c:b3:fc:89:93:db:9c:a1:56:7c:a0:5a:f9:37:7e:a0:
         81:dd:48:b4:a4:40:25:95:17:f3:00:0f:72:4d:9f:ca:9e:e0:
         c0:a1:d4:58:67:21:11:29:4c:97:0d:c6:38:db:c5:f8:80:98:
         1b:77:12:7c:7e:0c:bc:cc:11:d7:79:7e:45:d5:69:ae:5c:6d:
         a6:8a:a9:6c:c2:22:90:0d:74:a4:c5:af:56:72:cd:46:38:40:
         32:a9:05:29:5b:28:b9:fd
```

### Criar certificado intermediário no AWS Private CA

Agora que já temos a nossa CA root externa, a ideia é utiliza-la para assinar a CA intermediária (subordinada) que iremos criar no AWS Private CA, essa CA intermediária será responsável por emitir os certificados para uso final.

> **Observação 1**: A criação de CA na AWS tem um custo de 400 doláres mensal, mas para o primeira CA o custo é insento no primeiro mês.

> **Observação 2**: A chave privada da CA fica guardada somente na AWS, o que é uma vantagem de segurança pois não há o risco da mesmo vazar e ter de ser revogada, em contrapartida não há uma forma de exporta-lá.

Para gerar uma CA intermediária, é necessário primeiramente gerar sua requisição de assinatura de certificado (CSR) para que esse possa ser assinado pela CA root.

No AWS Private CA selecionamos o tipo subordinada e preenchemos as informações do certificado, assim como fizemos para gerar a CA root, normalmente preenchemos as mesmas informações para ambas com exceção do campo *Common Name* (CN).

![Criar CA subordinada](https://miro.medium.com/v2/format:webp/1*Zw1eA7dQmHdYlSUQCntiPw.jpeg)

Além disso devemos selecionar o tipo de algoritmo de chave privada (RSA ou ECDSA) e opcionalmente a configuração de revogação de certificado por CRL ou OCSP.

Feito isso, irá gerar um novo arn para este recurso AWS e estará no status *Pending Certificate*, porque a requisição para gerar o certificado (CSR) foi criada mas ainda não foi assinada por uma CA root para emitir o certificado desta CA intermediária.

![CA privada criada com status pendente para instalação](https://miro.medium.com/v2/format:webp/1*vgT8GEU6uKgnmZRHaO1dAA.jpeg)

Para assinarmos o CSR com a nossa root externa devemos selecionar o recurso e ir a opção *"Install CA certificate"*.


![Instalar a CA - assinar a CA intermediária com a CA root](https://miro.medium.com/v2/format:webp/1*8NKzxEj49sCauE-BUPhTvQ.jpeg)

Selecionamos então se está CA intermediária será assinada por uma CA já existente no AWS PCA ou se será por uma CA externa, no nosso caso será CA externa.

![Assinar a CA subordinada com a CA root externa](https://miro.medium.com/v2/format:webp/1*rI_467TfcXc-_PR-Lw_p5w.jpeg)

A AWS irá disponibilizar uma requisição de assinatura de certificado (CSR).

![CSR da CA emitido pela AWS](https://miro.medium.com/v2/format:webp/1*p6Zb0bv7dM4flqxX2FMc1Q.jpeg)

### 1. Copie o conteúdo do CSR disponibilizado pela AWS para o arquivo `intermediate-request.csr`

### 2. Preencha o arquivo `intermediate-openssl.cnf` com o seguinte conteúdo:

```
[ intermediate_ca_ext ]
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always, issuer:always
basicConstraints = critical,CA:true,pathlen:0
keyUsage = critical,keyCertSign,digitalSignature,cRLSign
```

### 3. Gere o certificado da CA intermediária

```
openssl x509 -req -days 3650 -extfile intermediate-openssl.cnf -extensions intermediate_ca_ext -in intermediate-request.csr -CA root-certificate.pem -CAkey root-encrypted.key -passin file:root-password-encrypted.txt -CAcreateserial -out intermediate-certificate.pem
```

Para emitirmos o certificado temos como base as informações da solicitação de assinatura (CSR) disponibilizado pela AWS `intermediate-request.csr` assinando o pela CA root `root-certificate.pem` e sua chave privada criptografada `root-encrypted.key` junto a senha para descriptografá-la `root-password-encrypted.txt`.

Também passamos as configurações de extensões deste certificado intermediário com base no perfil `intermediate_ca_ext` do arquivo `intermediate-openssl.cnf`, além disso a quantidade de dias que o certificado será válido, nesse caso 10 anos (3650 dias).

Definimos também o número serial aleatório usando o argumento `-CAcreateserial` e dessa forma geramos o certificado da CA intermediária `intermediate-certificate.pem`.

Com o certificado gerado, basta importar diretamente no AWS Private CA os respectivos campos:

- Certificate body: Certificado da CA intermediária  `intermediate-certificate.pem`.
- Certificate chain: Certificado da CA root  `root-certificate.pem`.

![Importar o corpo e cadeia do certificado](https://cdn-images-1.medium.com/v2/resize:fit:880/1*l5agdqJn5gRQxD59AGfQnQ.jpeg)

Com os certificados importados basta selecionar a opção *"Install CA"*. Dessa forma teremos uma cadeia de autoridade de certificação (CA) para emitir certificados privados na AWS via ACM.