---
id: explainable_ai
aliases: []
tags:
  - master
  - explainable_ai
---

# Explainable AI

Si tratta di un ambito di ricerca volto a spiegare scelte o predizioni
effettuate da modelli di Machine Learning, chiamati _black box_.

Di questi modelli si conosce ovviamente funzionamento e architettura ma quando
si tratta di capire perché abbiano fornito una determinata risposta, diventa
praticamente impossibile muoversi all'interno del mare di parametri che hanno
portato a quella risposta.

Quando però si ha a che fare con l'etica, la salute e la sicurezza diventa
necessario dare delle spiegazioni. Ecco che si aprono principalmente due strade
possibili:

- Creazione di modelli predittivi le cui scelte siano facilmente interpretabili
  e quindi _spiegabili_.
- Implementazione di metodi che riescano a cogliere il motivo di determinate
  scelte e predizioni effettuate da modelli _black box_.

Possiamo inoltre suddividere le tecniche di XAI in due macro-categorie:

- **Globali**: tecniche volte a comprendere come il modello effettui predizioni
  in generale sfruttando tutti i dati a disposizione contemporaneamente.
- **Locali**: tecniche volte a fornire spiegazioni a predizioni effettuate su
  istanza specifiche del dataset. Per le tecniche di tipo locale è inoltre
  possibile fornire regole **fattuali**, che spiegano il perché di una determinata
  scelta, e regole **controfattuali**, che invece indicano come modificare i dati
  al minimo per cambiare la scelta effettuata dal modello.

## References
