# Adaptor

Het Adaptor Pattern converteert de interface van een klasse naar een andere interface die de client verwacht. Adapters zorgen ervoor dat klassen samenwerken. Zonder de adapters lukt dit niet vanwege incompatibele interfaces.

![image](./images/Adaptor.png)

# Builder

Gebruik het builder Pattern om de constructie van een product af te schermen en zorg dat je het in stappen kan construeren.

* De **Builder** klasse specifieert een abstracte interface voor de creatie van de onderdelen van het Product object.
* De **ConcreteBuilder** bouwt de onderdelen van het complexe object en gooit deze samen door implementatie van de Builder Interface. Het houdt een representatievan het object bij en biedt een interface voor het opvangen van het product.
* De **Director** klasse bouwt het complexe object gebruik makend van de interface van de Builder.
* Het **Product** stelt het complexe object voor dat gebouwd wordt.

Geeft de mogelijkheid om objecten in meerdere stappen en wisselende processen te maken, in tegenstelling tot éénstapsfactory.
Vaak gebruikt voor **samengestelde objecten**.

![image](./images/Builder.png)
![image](./images/Builder1.png)

# Command

het Command Pattern schermt een aanroep af door middel van een object, waarbij je verschillende aanroepen in verschillende objecten kan opbergen, in een queue zetten, of op schijf kunt bewaren. Ook undo-operaties kunnen worden ondersteund.

* Een actie wordt voorgesteld als een **object**.
* De commandszijn volledig **self-contained**.
* Nieuwe commandos kunnen eenvoudig worden toegevegd.

![image](./images/Command.png)
![image](./images/Command1.png)

## Macro-Command

![image](./images/Command-Macro.png)

# Composite

Het Composite Pattern stelt je in staat om objecten in boomstructuren samen te stellen om partwhole hiërarchiën weer te geven. Composite laat clients de afzonderlijke objecten of samengestelde objecten op uniforme wijze behandelen.

![image](./images/Composite.png)
![image](./images/Composite1.png)

# Abstract Factory

Het Abstract Factory Pattern levert een interface voor de vervaardiging van reeksen gerelateerde of onafhankelijke objecten zonder hun concrete klassen te specifieren.

* De **Abstract Factory** definieert de interface dat alle Concrete Factories moeten implementeren, die bestaat uit een set van methodes voor de creatie van producten.
* De **Concrete Factory** maakt eigen versie van elk Product. De Client gebruikt een van deze Factories zodat die nooit een Product object moet instantieren.
* De **Client** is geschreven met de Abstracte Factory en wordt tijdens runtime composed met een Concrete Factory.

We schrijven de code zodanig dat de **Client** de **Factory** gebruikt voor het maken van de producten. Door een verscheidenheid aan fabrieken krijgen we een verscheidenheid aan implementaties voor de producten. Maar de Clientcode blijft hetzelfde.

We voorzien een manier voor het maken van een familie producten, Code is losgekoppeld van de Factory waardoor een variatie van Factories kunnen geimplementeerd worden.

![image](./images/AbstractFactory.png)

```java
public interface PizzaIngredientFactory {

	public Dough createDough();
	public Sauce createSauce();
	public Cheese createCheese();
	public Veggies[] createVeggies();
	public Pepperoni createPepperoni();
	public Clams createClam();

}

public class NYPizzaIngredientFactory implements PizzaIngredientFactory {
	
	public Dough createDough() {
		return new ThinCrustDough();
	}

	public Sauce createSauce() {
		return new MarinaraSauce();
	}

	public Cheese createCheese() {
		return new ReggianoCheese();
	}

	public Veggies[] createVeggies() {
		Veggies veggies[] = { new Garlic(), new Onion(), new RedPepper() };
		return veggies;
	}

	public Pepperoni create Pepperoni() {
		return new SlicedPepperoni();
	}

	public Clams createClam() {
		return new FreshClams();
	}
}

public abstract class Pizza {

	String name;
	Dough dough;
	sauce sauce;
	Veggies veggies[];
	Cheese cheese;
	Pepperoni pepperoni;
	Clams clam;

	abstract void prepare();

	void bake() {
		System.out.println("Bake for 25 minutes at 350°");
	}

	void cut() {
		System.out.println("Cutting the pizza into diagonal slices");
	}

	void box() {
		System.out.println("Place pizza in official PizzaStore box");
	}

	void setName(String name) {
		this.name = name;
	}

	String getName() {
		return name;
	}
}

public class CheesePizza extends Pizza {
	PizzaIngredientFactory ingredientFactory;

	public CheesePizza(PizzaIngredientFactory ingredientFactory) {
		this.ingredientFactory = ingredientFactory;
	}

	void prepare() {
		System.out.println("Preparing " + name);
		dough = ingredientFactory.createDough();
		sauce = ingredientFactory.createSauce();
		cheese = ingredientFactory.createCheese();
	}
}

public class NYPizzaStore extends PizzaStore {

	protected Pizza createPizza(String item) {
		Pizza pizza = null;
		PizzaIngredientFactory ingredientFactory = new PizzaIngredientFactory();

		if (item.equals("cheese")) {
			pizza = new CheesePizza(ingredientFactory);
			pizza.setName("New York Style Cheese Pizza");
		} else if (item.equals("veggie")) {
			pizza = new VeggiePizza(ingredientFactory);
			pizza.setName("New York Style Veggie Pizza");
		} else if (item.equals("clam")) {
			pizza = new ClamPizza(ingredientFactory);
			pizza.setName("New York Style Clam Pizza");
		} else if (item.equals("pepperoni")) {
			pizza = new PepperoniPizza(ingredientFactory);
			pizza.setName("New York Style Pepperoni Pizza);
		}
		return pizza;
	}
}
```

# Factory Method

Het Factory Method Pattern definieert een interface voor het creëren van een object, maar laat de subklassen beslissen welke klasse er geïnstantieert wordt. De Factory Method draagt de instanties door aan de subklassen.

* De **Creator** 
	* Bevat de implementaties voor alle methodes voor manipulatie van het product.
	* Bevat abstracte Factory Methode die alle subklassen moeten implementeren.
* De **Concrete Creator** 
	* Implementeert de Factory Methode.
	* Is verantwoordelijk voor de creatie van Concrete Producten.
	* Is de enige klasse die kennis over de creatie heeft.
* Alle **Producten** moeten dezelfde interface implementeren zodat de klassen die de producten gebruiken hier naar kunnen refereren.

De **Abstracte Creator-Klasse** definieert een abstracte fabrieksmethode die door de subklassen geïmplementeerd wordt om producten te vervaardigen. 

* De verantwoordelijkheid voor het maken van de producten is verplaatst naar een methode die zich gedraagt als een Factory.
* De methode isoleert de Client van het weten welk soort concreet product gemaakt is.

![image](./images/FactoryMethod.png)
![image](./images/FactoryMethod1.png)

```java
public abstract class PizzaStore {

	public Pizza orderPizza(String type) {
		Pizza pizza;

		pizza = createPizza(type);

		pizza.prepare();
		pizza.bake();
		pizza.cut();
		pizza.box();

		return pizza;
	}

	abstract Pizza createPizza(String type);
}

public class NYPizzaStore extends PizzaStore {
	
	Pizza createPizza(String item) {
		if (item.equals("cheese")) {
			return new NYStyleCheesePizza();
		} else if (item.equals("veggie")) {
			return new NYStyleVeggiePizza();
		} else if (item.equals("clam")) {
			return new NYStyleClamPizza();
		} else if (item.equals("pepperoni")) {
			return new NYStylePepperoniPizza();
		} else return null;
	}
}
```

# Simple Factory

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

		if (type.equals("cheese")) {
			pizza = new CheesePizza();
		} else if (type.equals("pepperoni")) {
			pizza = new PepperoniPizza();
		} else if (type.equals("clam")) {
			pizza = new ClamPizza();
		} else if (type.equals("veggie")) {
			pizza = new VeggiePizza();
		}
		return pizza;
	}
}
```


# Iterator

Het Iterator Pattern voorziet ons van een manier voor sequentiële toegang tot de elementen van een aggregaatobject zonder de onderliggende representatie weer te geven.

* Iterator Pattern is gebaseerd op een Iterator interface.
* Een implementatie van Iterator weet hoe hij moet itereren door zijn specifieke lijst.
* De Client wordt losgekoppeld van de implementatie van de concrete klassen.

![image](./images/Iterator1.png)
![image](./images/Iterator2.png)

```java
public interface Iterator {
	boolean hasNext();
	Object next();
}

public class DinnerMenuIterator implements Iterator {
	MenuItem[] items;
	int position = 0;

	public DinnerMenuIterator(MenuItem[] items) {
		this.items = items;
	}

	public MenuItem next() {
		MenuItem menuItem = items[position];
		position = position + 1;
		return menuItem;
	}

	public boolean hasNext() {
		if (position >= items.length || items[position] == null) {
			return false;
		} else {
			return true;
		}
	}
}

public class DinnerMenu {
	static final int MAX_ITEMS = 6;
	int nuberOfItems = 0;
	MenuItem[] menuItems;

	public Iterator createIterator() {
		return new DinnerMenuIterator(menuItems);
	}
}

public class Waitress {
	PancakeHouseMenu pancakeHouseMenu;
	DinerMenu dinerMenu;

	public Waitress(PancakeHouseMenu pancakeHouseMenu, DinerMenu dinerMenu) {
		this.pancakeHouseMenu = pancakeHouseMenu;
		this.dinerMenu = dinerMenu;
	}

	public void printMenu() {
		Iterator pancakeIterator = pancakeHouseMenu.createIterator();
		Iterator dinerIterator = dinerMenu.createIterator();

		System.out.println("MENU\n----\nBREAKFAST);
		printMenu(pancakeIterator);
		System.out.println("\nLUNCH");
		printMenu(dinerIterator);

	}

	public void printMenu(Iterator iterator) {
		while (iterator.hasNext()) {
			MenuItem menuItem = iterator.next();
			System.out.print(menuItem.getName() + ", ");
			System.out.print(menuItem.getPrice() + " -- ");
			System.out.println(menuItem.getDescription());
		}
	}
}
```

## Na opschonen met java.util.iterator en Menu Interface.

* De **PancakeHouseIterator** klasse wordt verwijderd en de create methode wordt aangepast.
* In de **DinerMenuIterator** klasse wordt een remove methode toegevoegd.

Hierdoor kent de Client enkel Menu en Iterator, de Client is losgekoppeld van de implementaties van Menus dus kan er een Iterator gebruikt worden om te itereren over elke menulijst.

![image](./images/Iterator3.png)

```java
public class PancakeHouseMenu {
	public Iterator<MenuItem> createIterator() {
		return menuItems.iterator();
	}
}

public class DinerMenuIterator implements Iterator {
	
	public void remove () {
		
		if (position <= 0) {
			throw new IllegalSateException("Kan niet verwijderen");
		}
		if (list[position-1] != null) {
			for (int i = position-1; i < (list.length-1) {
				list[i] = list[i+1];
			}
			list[list.length-1] = null;
		}
	}
}

public interface Menu {
	public Iterator<MenuItem> createIterator();
}
```
# Proxy

Het Proxy Pattern zorgt voor een surrogaat of plaatsvervanger voor een ander object om de toegang hiertoe te controleren.

Structureel gelijk aan **Decorator** maar de doelstellingen veranderen:

* Decorator voegt gedrag toe aan een object.
* Proxy regelt de toegang.

![image](./images/Proxy.png)

# Singleton

Het Singleton Pattern garandeert dat een klasse slechts één instantie heeft, en biedt een globaal toegangspunt ernaartoe.

![image](./images/Singleton1.png)

# Template Method

Het Template Method Pattern definieert het skelet van een algoritme in een methode, waarbij sommige stappen aan subklassen worden overgelaten. De Template Method laat subklassen bepaalde stappen in een algoritme herdefiniëren zonder de structuur van het algoritme te veranderen.

De Template Methode maakt gebruik van **primitieve methoden** om een algoritme te implementeren. Deze is echter ontkoppeld van de feitelijke implementatie van deze methoden.

![image](./images/Template.png)
![image](./images/Template1.png)

## Hook

![image](./images/Template-Hook.png)

