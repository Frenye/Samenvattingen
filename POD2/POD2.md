# 1. Gebeurtenissen en hun kansen

## 1.1 Inleiding

Kansrekening houdt zich bezig met studie van gebeurtenissen of toevalsverschijnselen. Dit zijn verschijnselen waarvan de individuele uitkomsten onzeker zijn, maar waar bij een groot aantal herhalingen een regelmatige verdeling van relatieve frequenties ontstaat.

De kans komt overeen met de relatieve frequentie op lange termijn

Onderdelen: 

1. Opstellen lijst mogelijke uitkomsten
2. Bepalen van de kans van elke uitkomst

## 1.2 Universum of uitkomstenruimte

Het universum of uitkomstenruimte is de verzameling van alle mogelijke uitkomsten van dit experiment en wordt genoteerd met Ω. Elke uitkomst moet overeenkomen met juist één element.

Het moet dus mogelijk zijn om eenduidig aan te geven welk element van Ω zich heeft voorgedaan.

## 1.3 Gebeurtenissen

Een gebeurtenis is een deelverzameling van de uitkomstenruimte. Een enkelvoudige of elementaire gebeurtenis is een singleton, een samengestelde heeft cardinaliteit groter dan 1.

Gebeurtenissen die geen gemeenschappelijke uitkomsten hebben noemt men disjunct. Deze kunnen dus nooit samen voorkomen.

Unie of doorsnee van gebeurtenissen zijn eveneens gebeurtenissen.

## 1.4 Kansen en kansruimte

Kans of waarschijnlijkheid drukt uit hoe waarschijnlijk het is dat deze gebeurtenis voorkomt. We noteren deze kans als P(A)

Het toekennen van kansen aan gebeurtenissen dient aan volgende regels te voldoen:

1. Kansen zijn steeds positief
2. Uitkomstenruimte heeft kans 1
3. Wanneer A en B disjuncte gebeurtenissen zijn dan is P(A ∪ B) = P(A) + P(B)

Wanneer de functie P aan volgende eigenschappen voldoet is dit een kansruimte.

Eigenschappen:

1. Voor elke gebeurtenis A geldt dat P(Ā) = 1 - P(A)
2. De onmogelijke gebeurtenis heeft kans 0: P(∅) = 0
3. Als A ⊆ B, dan is P(A) ≤ P(B); i.h.b geldt P(A) = P(B) - P(B \ A)
4. De uitgebreide somregel is: P(A ∪ B) = P(A) + P(B) - P(A ∩ B)

### 1.4.1 Eindig universum

Als de uitkomstenverzameling eindig is dan moet de som van de kansen van alle elementaire gebeurtenissen gelijk zijn aan 1.

Wanneer de elementaire gebeurtenissen allemaal even waarschijnlijk zijn dan bekomt men de formule van Laplace:

![image](./images/Laplace.png)

## 1.5 Voorwaardelijke kansen en (on)afhankelijkheid van gebeurtenissen

Uitdrukkingen zijn vaak van de vorm

> Als B voorkomt, dan is de waarscheinlijkheid dat A voorkomt gelijk aan p.

Kans op A gegeven B: 

![image](./images/VK.png)

Wanneer de waarschijnlijkheid niet veranderd kunnen we zeggen dat A en B onafhankelijk zijn. Gebeurtenissen zijn onafhankelijk als:

P(A ∩ B) = P(A)P(B)

Als A en B onafhankelijk zjn dan geldt 

P(A) = P(A|B)

#### Kettingregel

Wanneer A₁ tem An gebeurtenissen zijn waarvoor

P(A₁ ∩ A₂ ∩ ... ∩ An-1) > 0

Dan geldt

![image](./images/KR.png)

### 1.5.2 Regel van Bayes

Gegeven een gebeurtenis A met n elkaar uitsluitende oorzaken Bi geldt voor elke j dat:

![image](./images/RvB.png)

