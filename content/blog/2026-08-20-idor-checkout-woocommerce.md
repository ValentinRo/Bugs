---
title: Cum am găsit un IDOR în checkout-ul unui magazin WooCommerce
date: 2026-08-20
category: Web · Critical
summary: Un parametru de comandă needitat permitea citirea facturilor altor clienți. Pași de reproducere, impact și remediere.
readingTime: 6 min citire
draft: false
---

## Context

Acesta este un articol demonstrativ, generat împreună cu configurarea editorului.
Șterge-l după ce publici primul articol real.

## Descoperire

Parametrul `order_id` din endpoint-ul de facturare nu verifica proprietarul comenzii:

```
GET /wp-json/wc/v3/orders/1042/invoice
```

Schimbarea id-ului returna facturi ale altor clienți, inclusiv nume, adresă și valoare comandă.

## Impact

Expunere de date personale ale clienților (GDPR), fără autentificare privilegiată.

## Remediere

- Validează proprietarul resursei pe server, nu în interfață.
- Adaugă teste automate de autorizare pentru fiecare endpoint care returnează date de client.
