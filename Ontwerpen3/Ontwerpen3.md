#Adaptor

Het Adaptor Pattern converteert de interface van een klasse naar een andere interface die de client verwacht. Adapters zorgen ervoor dat klassen samenwerken. Zonder de adapters lukt dit niet vanwege incompatibele interfaces.

![image](./images/Adaptor.png)

#Builder

Gebruik het builder Pattern om de constructie van een product af te schermen en zorg dat je het in stappen kan construeren.

* De **Builder** klasse specifieert een abstracte interface voor de creatie van de onderdelen van het Product object.
* De **ConcreteBuilder** bouwt de onderdelen van het complexe object en gooit deze samen door implementatie van de Builder Interface. Het houdt een representatievan het object bij en biedt een interface voor het opvangen van het product.
* De **Director** klasse bouwt het complexe object gebruik makend van de interface van de Builder.
* Het **Product** stelt het complexe object voor dat gebouwd wordt.

Geeft de mogelijkheid om objecten in meerdere stappen en wisselende processen te maken, in tegenstelling tot éénstapsfactory.
Vaak gebruikt voor **samengestelde objecten**.

![image](./images/Builder.png)
![image](./images/Builder1.png)

#Command

het Command Pattern schermt een aanroep af door middel van een object, waarbij je verschillende aanroepen in verschillende objecten kan opbergen, in een queue zetten, of op schijf kunt bewaren. Ook undo-operaties kunnen worden ondersteund.

* Een actie wordt voorgesteld als een **object**.
* De commandszijn volledig **self-contained**.
* Nieuwe commandos kunnen eenvoudig worden toegevegd.

![image](./images/Command.png)
![image](./images/Command1.png)

##Macro-Command

![image](./images/Command-Macro.png)

#Composite

Het Composite Pattern stelt je in staat om objecten in boomstructuren samen te stellen om partwhole hiërarchiën weer te geven. Composite laat clients de afzonderlijke objecten of samengestelde objecten op uniforme wijze behandelen.

![image](./images/Composite.png)
![image](./images/Composite1.png)

#Abstract Factory

Het Abstract Factory Pattern levert een interface voor de vervaardiging van reeksen gerelateerde of onafhankelijke objecten zonder hun concrete klassen te specifieren.

We schrijven de code zodanig dat de **Client** de **Factory** gebruikt voor het maken van de producten. Door een verscheidenheid aan fabrieken krijgen we een verscheidenheid aan implementaties voor de producten. Maar de Clientcode blijft hetzelfde.

![image](./images/AbstractFactory.png)

#Factory Method

Het Factory Method Pattern definieert een interface voor het creëren van een object, maar laat de subklassen beslissen welke klasse er geïnstantieert wordt. De Factory Method draagt de instanties door aan de subklassen.

De **Abstracte Creator-Klasse** definieert een abstracte fabrieksmethode die door de subklassen geïmplementeerd wordt om producten te vervaardigen.

![image](./images/FactoryMethod.png)

#Simple Factory

We nemen de code voor de creatie op en verplaatsen deze naar een ander object dat alleen maar het maken als taak zal hebben. Wanneer een methode een klasse retoruneert uit verschillende klassen met gelijkaardige superklasse.

* De **Client** gaat door de Factory voor instanties van het product.
* De **Factory** maakt instanties van het product, dit is het enige deel van de applicatie die refereert daar de concrete klassen van het product.
* De **product** klasse is abstract en heeft implementaties die overschreven kunnen worden.
* de **Concrete Producten** implementeren de Product klasse en kunnen gemaakt worden door de Factory.

![image](./images/SimpleFactory.png)

```java
public class SimplePizzaFactory {
public Pizza createPizza(String type){
	Pizza pizza = null;
}
}
```


#Iterator

Het Iterator Pattern voorziet ons van een manier voor sequentiële toegang tot de elementen van een aggregaatobject zonder de onderliggende representatie weer te geven.

* Iterator Pattern is gebaseerd op een Iterator interface.
* Een implementatie van Iterator weet hoe hij moet itereren door zijn specifieke lijst.

![image](./images/Iterator1.png)
![image](./images/Iterator2.png)

Na opschonen met java.util.iterator

![image](./images/Iterator3.png)

#Proxy

Het Proxy Pattern zorgt voor een surrogaat of plaatsvervanger voor een ander object om de toegang hiertoe te controleren.

Structureel gelijk aan **Decorator** maar de doelstellingen veranderen:

* Decorator voegt gedrag toe aan een object.
* Proxy regelt de toegang.

![image](./images/Proxy.png)

#Singleton

Het Singleton Pattern garandeert dat een klasse slechts één instantie heeft, en biedt een globaal toegangspunt ernaartoe.

![image](./images/Singleton1.png)

#Template Method

Het Template Method Pattern definieert het skelet van een algoritme in een methode, waarbij sommige stappen aan subklassen worden overgelaten. De Template Method laat subklassen bepaalde stappen in een algoritme herdefiniëren zonder de structuur van het algoritme te veranderen.

De Template Methode maakt gebruik van **primitieve methoden** om een algoritme te implementeren. Deze is echter ontkoppeld van de feitelijke implementatie van deze methoden.

![image](./images/Template.png)
![image](./images/Template1.png)

##Hook

![image](./images/Template-Hook.png)

