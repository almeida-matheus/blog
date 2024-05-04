+++
title = "Tipos de formatos de certificados SSL"
date = 2024-04-30T00:48:11-03:00
draft = false
author = "Matheus Almeida Costa"
description = "Confira ps diferentes tipos de formatos de certificados digitais e suas características e usos específico"
categoria = [
    "Segurança",
]
+++

## Introdução

Certificados digitais SSL/TLS são como credenciais de identidade digitais. Comumente são utilizado para proteger a comunicação de dados transferidos pela internet através da criptografia e também da autenticação.

Existem diferentes tipos de formatos de certificados digitais, cada um com suas características e usos específicos:

### PEM (Privacy Enhanced Mail)
É um formato de arquivo comum para armazenar certificados SSL (X.509). Arquivos PEM podem conter a chave pública e o certificado pública e a autoridade de certificação em texto plano.

Geralmente têm extensões como `.pem`, `.crt`, `.cer` ou `.key`.

O formato do conteúdo pode ser base-64 (string) com header e footer incluídos

### DER (Distinguished Encoding Rules)

Segue a mesma lógica de certificados PEM, algumas vezes são até mesmo incluídos na mesmo grupo, o que difere o DER dos demais é o formato de arquivo binário (ASN.1 DER).

Geralmente sua extensão é a `.der`.

### PFX (Personal Information Exchange)

Também conhecido como formato PKCS#12. É um formato de arquivo para armazenar vários objetos de criptografia como um único arquivo normalmente criptografado com uma senha. É comumente usado para agrupar uma chave privada com seu certificado e até certificados de autoridades de certificação.

Certificados PFX são frequentemente usados para exportar e importar certificados e chaves privadas em servidores Windows.

Geralmente têm extensões como `.pfx` e `.p12`.

O formato do conteúdo é binário.

### JKS (Java KeyStore)

É um formato de armazenamento de chaves usado por Java para armazenar chaves de criptografia, certificados e autoridades de certificação em um único arquivo semelhante ao formato PFX.

São comumente usados em aplicativos Java e servidores web que executam na plataforma Java.

Arquivos JKS geralmente têm a extensão `.jks`.

## Conclusão

- Tanto o **PFX** quanto o **PEM** e **JKS** podem armazenar cadeias de certificados inteiras: chaves públicas, chaves privadas e certificados raiz (CA).
- Você nunca deve divulgar sua chave privada. Vale ressaltar que a chave privada pode estar contida em arquivos `.pfx` e `.jks` mas comumente protegidas por uma criptografia com senha.

---

Este artigo foi originalmente publicado no [Medium](https://almeida-matheus.medium.com/tipos-de-formatos-de-certificados-ssl-54472a1f0cfc).